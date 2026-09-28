# 評価と再現実験の設計

[調査トップ](README.md) / [資料台帳](sources.md)

## ベンチマークが測るもの

| benchmark | タスク・規模の位置づけ | 有用な理由 | この評価だけでは不足する部分 |
|---|---|---|---|
| LoCoMo | 長い複数セッション対話、QA・要約・画像付き対話 | 関係・時系列・話者を保持できるか | 原論文の 50 対話と後続研究で使う `locomo10` を区別。adversarial を外した集計を混ぜない。[P-LOCOMO] [D-LOCO-CODE] |
| LongMemEval | 500 問。S は問題当たり約 115k tokens、M は約 1.5M | 抽出、多セッション、時間、更新、回答保留 | 5 能力と質問タイプ数は異なる。単一正答の採点だけで更新履歴の正しさは測れない。[P-LME] |
| ConvoMem | 75,336 QA、user/assistant facts、嗜好、時間変化、暗黙関係、abstention | 小さい履歴では full context が強いという対照を与える | 「150 会話」という境界は著者の条件に依存し、普遍的な切り替え閾値ではない。[P-CONVOMEM] |
| BEAM | 長大で一貫した会話からの 100 conversations / 2,000 validated questions | 長期の narrative と複数能力を評価 | 類似名の別論文 BEAM と区別し、context-length / ability の条件を固定。[P-BEAM] |
| MemoryAgentBench | 増分の multi-turn 入力。retrieval、test-time learning、long-range understanding、selective forgetting | 文書を一度渡すだけでない蓄積能力 | 元データ・task ごとの性質と採点方法を保持。[P-MAB] |
| HaluMem | extraction、updating、QA の操作別評価。Medium/Long | どの段階で誤情報が入るかを定位 | operation を外から観察できない API は公平な adapter が必要。[P-HALU] |
| MemoryArena | 相互依存する複数セッションでの行動タスク | 過去の結果を次の行動に利用できるか | environment、tool、行動モデルの影響を分離。[P-ARENA] |
| PersonaMem | 動く人物像と個人化 | 明示的に聞かれない情報の活用 | 人物像の推定誤り・拒否・撤回は別途測る。[P-PERSONA] |
| EverMemBench | 複数人・グループ・長大履歴 | speaker、scope、時間、暗黙情報の検索 | 合成データと現実の利用者分布の差。[P-EVERBENCH] |
| LoCoMo-Plus | 後の質問と語彙が直接一致しない潜在制約 | 単純 recall で見えない失敗 | 推定の適切性を別に評価。[P-LOCOPLUS] |
| NoLiMa | literal overlap の少ない長文 retrieval | 単語一致型 needle test の補完 | 知識更新や消去の評価ではない。[P-NOLIMA] |

## 公開性能値をどう読むか

以下は著者・提供者の報告であり、今回再測定した結果ではない。

| 出典・条件 | 報告値の例 | 読み取れること／読み取れないこと |
|---|---|---|
| Mem0 論文、LoCoMo の四カテゴリ、主な処理 GPT-4o-mini、Table 2 | J score: Mem0 **66.88%**、graph **68.44%**、full context **72.90%**。total p95: 1.440 / 2.590 / 17.117 秒 | graph は基礎方式より +1.56 percentage points。full context より速いが、表の正答指標は full context が上。現行 OSS のスコアではない。[P-M0] |
| Zep 論文、LongMemEval-S、Table 2 | GPT-4o: full context **60.2%** → Zep **71.2%**。平均 context 約 115k → 1.6k | +11.0 percentage points、相対約18.3%（丸めた表値から計算）。全 memory system との統一比較ではない。[P-ZEP] |
| SimpleMem、LoCoMo、GPT-4.1-mini、Table 1 | average F1: Mem0 **34.20** → SimpleMem **43.24** | +9.04 points、相対約26.4%。F1 を上記の J score と並べない。[P-SIMPLE] |
| Hindsight 論文、LongMemEval、GPT-OSS-20B の抽出・reflection | overall accuracy **83.6%**。同 backbone の full-context baseline **39%** | judge は GPT-OSS-120B と記載され、LongMemEval 原論文の GPT-4o judge とは異なる。数値だけで他方式の順位にしない。[P-HIND] |
| Mastra 公式 research、LongMemEval-S、500 問 | GPT-4o 回答で **84.23%**、GPT-5-mini 回答で **94.87%** | ingestion は Gemini 2.5 Flash。異なる回答器の値を memory algorithm の差だけに帰属しない。提供者による評価。[D-MASTRA] |

Mem0 論文は adversarial questions を評価から除いている。「忘れてほしい」「知らないと答えるべき」まで高品質と結論しない。同論文の OpenAI 比較も、当時抽出した記憶を使う固有の手順であり、現在の ChatGPT 全体の性能として解釈しない。[P-M0]

Zep 論文の GPT-4o-mini は全体で改善しても、knowledge-update では full context 76.9% に対し Zep 74.4% と報告する。平均改善は全能力の改善を意味しない。[P-ZEP]

## 二通りの比較を行う

**アルゴリズム比較。** 抽出器・回答器・embedding・取得トークン予算をなるべく固定し、記憶形式や更新方針の効果を調べる。再ランキングや agentic retrieval の追加呼び出しも同じ費用枠で数える。

**完成品比較。** 提供者推奨の構成を使い、全体の品質・遅延・費用・運用の複雑さを測る。モデルや非公開処理が異なるため、勝敗を純粋なメモリー方式の因果効果とは呼ばない。

必須の基準線は、記憶なし、直近窓、全履歴（入る場合）、全文検索、dense retrieval、hybrid retrieval、短い profile、Markdown＋検索、観察ログ型、代表的グラフ型。oracle evidence は生成器の上限の参考として用いる。

## 再現のために固定する項目

```yaml
run:
  dataset: {name: longmemeval_s, revision: "要固定", split: test}
  ingestion: {order: chronological, visibility: past_only, wait_policy: job_completed}
  system: {repository: "対象repo", commit: "完全SHA", config_hash: "要記録"}
  extraction: {model: "固定ID", prompt_hash: "要記録", temperature: 0}
  retrieval: {embedding: "固定ID", top_k: 10, context_token_budget: 4000}
  answer: {model: "固定ID", prompt_hash: "要記録"}
  judge: {model: "固定ID", rubric_revision: "要記録"}
  accounting: {ingestion: true, consolidation: true, query: true, retries: true}
```

これは計測仕様の例であり、上記設定で実験したという意味ではない。検索候補数だけを同じにしても、記憶一件が一文か一ページかで情報量は違うため、token budget も合わせる。

入力履歴の未来部分や評価質問を取り込み側へ見せない。同じ会話から多数の質問を作る場合、train/test を質問単位だけで分けず、会話・人物・期間単位の漏えいも調べる。背景統合があるサービスは完了条件を記録し、固定の数秒待ちで「同じ条件」としない。冷たい cache と温まった cache を分ける。

## 独自に追加する更新シナリオ

| 試験 | 合格条件 |
|---|---|
| 遅れて届いた訂正 | 過去の有効状態と、訂正前の認識を別々に答えられる |
| 同姓同名／複数言語別名 | 誤統合を防ぎ、統合の取り消し後も根拠が失われない |
| 複数値の併存 | 兼業・複数住居を単純に旧値の矛盾として消さない |
| 仮定・予定・否定・引用 | 実現した事実へ無条件に昇格しない |
| 一つのソースの撤回 | 独立した別の根拠がある主張を必要以上に削除しない |
| schema の改訂 | 旧 predicate のデータを新 schema へ写像し、由来を保つ |
| 二重送信・並行更新 | 同じイベントで二重に強化されず、lost update がない |
| tenant と観測者 | 利用可能でない原資料に由来する要約も漏れない |
| 削除後の再構築 | cache / embedding / graph の再構築で削除済み情報が復活しない |
| 検索不能・知らない質問 | 誤答を作らず、根拠不足を示す |
| 日本語特有の表現 | 省略主語、敬称、姓・名、和暦、相対日付、全半角を扱える |
| 複数モダリティ | OCR／音声転記の誤りと、元の画像領域・時間範囲を追える |

## 指標

書き込みは assertion の precision/recall、実体同定の誤統合率、更新操作の適合率、出典 span の正しさ。検索は recall@k と stale-fact rate、権限違反率、必要な根拠集合の回収率。回答は QA 正答、引用の支持、unknown の適切さ、棄却率と誤答率のトレードオフ。行動は downstream task success と負の転移を別に測る。

費用は一回の回答だけでなく、取り込み、統合、再埋め込み、削除、再試行を含める。LLM / embedding / rerank の呼び出し数、tokens、p50/p95、検索可能になるまでの遅延を保存する。書き込みで節約し検索で高い費用を払う方式と、その逆を比較できるようにする。

LLM judge は人手で層化サンプルを二重採点し、採点器変更への頑健性を確認する。原 LongMemEval は専用 rubric と人手一致の検討を含む。Ragas の faithfulness は与えた context への忠実性であり、その context が現実に正しいことまでは保証しない。[P-LME] [D-RAGAS]

## 関連ドキュメント

- [研究の系譜と各論文の位置づけ](papers.md)
- [検証仮説と実験の順序](design-directions.md)

[P-LOCOMO]: https://arxiv.org/abs/2402.17753v1
[D-LOCO-CODE]: https://github.com/snap-research/locomo
[P-LME]: https://arxiv.org/abs/2410.10813v2
[P-CONVOMEM]: https://arxiv.org/abs/2511.10523v1
[P-BEAM]: https://arxiv.org/abs/2510.27246v2
[P-MAB]: https://arxiv.org/abs/2507.05257v4
[P-HALU]: https://arxiv.org/abs/2511.03506v3
[P-ARENA]: https://arxiv.org/abs/2602.16313v2
[P-PERSONA]: https://arxiv.org/abs/2504.14225v2
[P-EVERBENCH]: https://arxiv.org/abs/2602.01313v3
[P-LOCOPLUS]: https://arxiv.org/abs/2602.10715v1
[P-NOLIMA]: https://arxiv.org/abs/2502.05167v3
[P-M0]: https://arxiv.org/abs/2504.19413v1
[P-ZEP]: https://arxiv.org/abs/2501.13956v1
[P-SIMPLE]: https://arxiv.org/abs/2601.02553v3
[P-HIND]: https://arxiv.org/abs/2512.12818v1
[D-MASTRA]: https://mastra.ai/research/observational-memory
[D-RAGAS]: https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/
