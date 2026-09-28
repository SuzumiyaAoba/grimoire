# 評価と再現実験の設計

[調査トップ](README.md) / [資料台帳](sources.md)

## ベンチマークが測るもの

| benchmark        | タスク・規模の位置づけ                                                                                  | 有用な理由                             | この評価だけでは不足する部分                                                                     |
| ---------------- | -------------------------------------------------------------------------------------------- | --------------------------------- | ---------------------------------------------------------------------------------- |
| LoCoMo           | 長い複数セッション対話、QA・要約・画像付き対話                                                                     | 関係・時系列・話者を保持できるか                  | 原論文の 50 対話と後続研究で使う `locomo10` を区別。adversarial を外した集計を混ぜない。[P-LOCOMO] [D-LOCO-CODE] |
| LongMemEval      | 500 問。S は問題当たり約 115k tokens、M は約 1.5M                                                        | 抽出、多セッション、時間、更新、回答保留              | 5 能力と質問タイプ数は異なる。単一正答の採点だけで更新履歴の正しさは測れない。[P-LME]                                    |
| ConvoMem         | 75,336 QA、user/assistant facts、嗜好、時間変化、暗黙関係、abstention                                       | 小さい履歴では full context が強いという対照を与える | 「150 会話」という境界は著者の条件に依存し、普遍的な切り替え閾値ではない。[P-CONVOMEM]                                |
| BEAM             | 長大で一貫した会話からの 100 conversations / 2,000 validated questions                                   | 長期の narrative と複数能力を評価            | 類似名の別論文 BEAM と区別し、context-length / ability の条件を固定。[P-BEAM]                         |
| MemoryAgentBench | 増分の multi-turn 入力。retrieval、test-time learning、long-range understanding、selective forgetting | 文書を一度渡すだけでない蓄積能力                  | 元データ・task ごとの性質と採点方法を保持。[P-MAB]                                                    |
| HaluMem          | extraction、updating、QA の操作別評価。Medium/Long                                                    | どの段階で誤情報が入るかを定位                   | operation を外から観察できない API は公平な adapter が必要。[P-HALU]                                 |
| MemoryArena      | 相互依存する複数セッションでの行動タスク                                                                         | 過去の結果を次の行動に利用できるか                 | environment、tool、行動モデルの影響を分離。[P-ARENA]                                             |
| PersonaMem       | 動く人物像と個人化                                                                                    | 明示的に聞かれない情報の活用                    | 人物像の推定誤り・拒否・撤回は別途測る。[P-PERSONA]                                                    |
| EverMemBench     | 複数人・グループ・長大履歴                                                                                | speaker、scope、時間、暗黙情報の検索          | 合成データと現実の利用者分布の差。[P-EVERBENCH]                                                     |
| LoCoMo-Plus      | 後の質問と語彙が直接一致しない潜在制約                                                                          | 単純 recall で見えない失敗                 | 推定の適切性を別に評価。[P-LOCOPLUS]                                                           |
| NoLiMa           | literal overlap の少ない長文 retrieval                                                             | 単語一致型 needle test の補完             | 知識更新や消去の評価ではない。[P-NOLIMA]                                                          |

## 公開性能値をどう読むか

以下は著者・提供者の報告であり、今回再測定した結果ではない。

| 出典・条件                                               | 報告値の例                                                                                                 | 読み取れること／読み取れないこと                                                                                        |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Mem0 論文、LoCoMo の四カテゴリ、主な処理 GPT-4o-mini、Table 2      | J score: Mem0 **66.88%**、graph **68.44%**、full context **72.90%**。total p95: 1.440 / 2.590 / 17.117 秒 | graph は基礎方式より +1.56 percentage points。full context より速いが、表の正答指標は full context が上。現行 OSS のスコアではない。[P-M0] |
| Zep 論文、LongMemEval-S、Table 2                        | GPT-4o: full context **60.2%** → Zep **71.2%**。平均 context 約 115k → 1.6k                               | +11.0 percentage points、相対約18.3%（丸めた表値から計算）。全 memory system との統一比較ではない。[P-ZEP]                          |
| SimpleMem、LoCoMo、GPT-4.1-mini、Table 1               | average F1: Mem0 **34.20** → SimpleMem **43.24**                                                      | +9.04 points、相対約26.4%。F1 を上記の J score と並べない。[P-SIMPLE]                                                  |
| Hindsight 論文、LongMemEval、GPT-OSS-20B の抽出・reflection | overall accuracy **83.6%**。同 backbone の full-context baseline **39%**                                 | judge は GPT-OSS-120B と記載され、LongMemEval 原論文の GPT-4o judge とは異なる。数値だけで他方式の順位にしない。[P-HIND]                 |
| Mastra 公式 research、LongMemEval-S、500 問              | GPT-4o 回答で **84.23%**、GPT-5-mini 回答で **94.87%**                                                       | ingestion は Gemini 2.5 Flash。異なる回答器の値を memory algorithm の差だけに帰属しない。提供者による評価。[D-MASTRA]                  |

Mem0 論文は adversarial questions を評価から除いている。「忘れてほしい」「知らないと答えるべき」まで高品質と結論しない。同論文の OpenAI 比較も、当時抽出した記憶を使う固有の手順であり、現在の ChatGPT 全体の性能として解釈しない。[P-M0]

Zep 論文の GPT-4o-mini は全体で改善しても、knowledge-update では full context 76.9% に対し Zep 74.4% と報告する。平均改善は全能力の改善を意味しない。[P-ZEP]

## 二通りの比較を行う

**アルゴリズム比較。** 抽出器・回答器・embedding・取得トークン予算をなるべく固定し、記憶形式や更新方針の効果を調べる。再ランキングや agentic retrieval の追加呼び出しも同じ費用枠で数える。

**完成品比較。** 提供者推奨の構成を使い、全体の品質・遅延・費用・運用の複雑さを測る。モデルや非公開処理が異なるため、勝敗を純粋なメモリー方式の因果効果とは呼ばない。

メモリー方式の基準線は、記憶なし、直近窓、全履歴（入る場合）、全文検索、dense retrieval、hybrid retrieval、短い profile、Markdown＋検索、観察ログ型、代表的グラフ型。oracle evidence は生成器の上限の参考として用いる。

## PageIndex を含む文書 QA の比較設計

PageIndex は、与えられた文書の中で章・節・ページを辿って根拠を探す検索層として評価する。メモリーへの事実抽出・訂正・忘却を評価するベンチマークとはタスクを分け、文書そのものへの質問応答を投入前の必須な意味抽出に依存させない。既知の文書をそのまま参照する経路と、文書集合から対象を選ぶ経路を分離する。[D-PI-START] [D-PI-OSS-BENCH]

PageIndex の公開 OSS 評価は、Flash が索引作成を拒否した文書を評価対象から外す条件を含む。したがって、適格文書だけの検索精度と、投入した全資料を対象にするシステム評価を混同しない。Flash の拒否条件・OCR 範囲は[PageIndex 調査](systems/pageindex.md)に記録されている。[D-PI-OSS-BENCH] [D-PI-FLASH] [C-PI-LOCAL]

**既知文書と文書集合の評価経路**

|                           | 比較対象                                                                      | 分けて報告する指標                                                         |
| ------------------------- | ------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| 既知文書 QA（質問に対象文書 ID が含まれる） | 原文書を直接取得して読む基準線、文書内 FTS＋metadata、PageIndex。dense は任意の追加比較                 | ページ／根拠候補の回収率、原ページ citation の支持率、回答正答率、棄却の適切さ                      |
| 文書集合 QA（対象文書 ID を与えない）    | 文書選択の metadata＋FTS と、選択後の文書内 FTS を最小基準線とし、dense は任意比較、PageIndex は文書内検索の候補 | 文書選択 Recall\@k、選ばれた文書内のページ根拠回収率、両段階を通した end-to-end 正答・citation 支持 |

対応する出典: PageIndex の文書内検索範囲 [D-PI-START] [C-PI-LOCAL]、公開 benchmark の対象条件 [D-PI-OSS-BENCH]。

文書集合 QA は「どの文書を選ぶか」と「選ばれた文書内のどのページを根拠にするか」を別々に採点する。文書選択を正解した質問だけに条件づけた内部検索の値も併記するが、全質問を分母にした end-to-end 値を主結果とし、前段階の誤りを隠さない。既知文書 QA では文書集合検索の成績を混ぜず、指定文書の直接取得を必ず基準に含める。

### 母数と失敗の記録

投入対象の全ファイルと評価質問を先に固定し、少なくとも次を分けて件数と率を報告する。

**文書と質問の母数・失敗を記録する**

|          | 記録する区分                              | 集計上の扱い                                             |
| -------- | ----------------------------------- | -------------------------------------------------- |
| 文書取り込み   | 全投入文書、索引可能、製品による拒否／非対応、解析・索引失敗、時間切れ | end-to-end では拒否と失敗も評価の分母に含める。索引可能文書だけの条件付き精度は別欄にする |
| 質問評価     | 固定した全質問、根拠がある質問、範囲外質問、未回答・棄却、実行エラー  | corpus-wide は拒否文書にしか根拠がない質問も含め、全対象質問を分母にした結果を出す    |
| fallback | OCR、別パーサー、FTS／直接取得などに移った文書・質問       | fallback 成功はシステム全体の結果に含める一方、使用した経路と追加費用を区別する       |

対応する出典: Flash が拒否する文書・評価対象の限定 [D-PI-OSS-BENCH] [D-PI-FLASH] [C-PI-LOCAL]。

Flash が拒否した文書を「対象外」にできるのは、その限定された適格集合での診断値を併記する場合だけとする。全体評価では「拒否」として残し、拒否文書を含めて質問を作った場合は回答不能を end-to-end の結果に反映する。スキャン PDF や画像主体の資料には OCR 等の fallback を用意する場合、その導入条件、失敗、誤認識、費用を含める。PageIndex Flash 自体を OCR 対応と仮定しない。[D-PI-OSS-BENCH] [D-PI-FLASH] [C-PI-LOCAL]

### 比較条件を二層に分ける

**共通解析での比較。** 同じ資料版、ページ境界、OCR／テキスト抽出結果、質問、索引・検索・回答モデルと版、judge、prompt、回答用 context budget をそろえ、検索層や根拠選択の違いを比較する。揃えられないモデルや処理は差分として記録し、その影響を方式単体の効果に帰属しない。PageIndex を共通のページ表現に載せられない場合は、その互換性の欠如を報告し、方式単体を同条件で比べたかのように扱わない。索引生成・質問時の追加 LLM 探索は、呼び出しごとの対象モデルと予算を明示して別途数える。

**native end-to-end 比較。** 各方式の推奨 parser、OCR、索引作成、検索、回答経路を使い、利用者が得る全体品質・対応範囲・遅延・費用・運用負荷を比較する。モデル、prompt、解析器、fallback が異なれば、差を PageIndex の検索方式だけの因果効果とは呼ばず、構成一式の比較として報告する。両比較とも資料の改訂版、モデル／parser の版、設定、失敗、retry を固定・記録する。

### 根拠・費用・採用ゲート

回答の citation は tree の要約だけでなく、原資料の版と実際の PDF ページに戻して検証する。PDF の物理ページと紙面の印刷ページ番号を区別し、引用 span が回答主張を支持するか、存在しないページや別版を指していないかを測る。少なくとも回答正答率、citation の原ページ解決率、citation support precision、根拠回収率、unsupported claim 率、適切な棄却率を別々に残す。評価 judge の判定だけでなく、層化した人手レビューでも支持関係を確認する。

費用は索引作成の一回限りの LLM 呼び出し・token・時間、文書追加／改訂時の再索引、問い合わせごとの探索呼び出し数、分岐 fanout、累積 context tokens、回答・rerank・retry、OCR fallback を分けて記録する。文書数・ページ数と問い合わせ数に応じた総費用と、索引費用を償却した費用を示し、p50/p95 latency と索引更新から利用可能になるまでの時間も測る。単一質問の料金だけで採用判断をしない。

採用前の必須ゲートは、権限で絞った文書 ID だけを検索層へ渡すこと、ACL 変更後に旧権限の利用者が結果・citation を再取得できないこと、削除が原文由来のページ本文・tree・cache・埋め込み・要約にも伝播して復活しないこと、並行再索引中に旧世代と新世代が混ざらず revoke 済み資料を返さないこと。いずれかの漏えい・削除復活があれば精度向上で相殺せず不採用とする。権限制御と文書指定を別に監査する。[C-PI-LOCAL] [C-PI-STORE] [C-PI-TOOLS]

精度向上率、許容費用、遅延の目標値はまだ実測していないため、現時点の採用基準値ではなく実験前に合意する目標とする。代表的な実資料で最小基準線（metadata＋FTS、既知文書なら直接取得）に対する再現可能な利得が確認でき、費用・遅延・更新運用が許容範囲に収まり、上記ゲートをすべて通過した場合に限って PageIndex を採用候補にする。差が不確実、対象外／拒否が多い、または安全ゲート未達なら既定の全文検索と fallback を維持する。PageIndex 調査ではコード・製品 API・ベンチマークの実行をしていないため、この文書の性能値や採用条件を達成済みの結果とは扱わない。[D-PI-OSS-BENCH] [D-PI-FINANCE] [D-PI-FINANCE-EVAL]

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

| 試験          | 合格条件                                        |
| ----------- | ------------------------------------------- |
| 遅れて届いた訂正    | 過去の有効状態と、訂正前の認識を別々に答えられる                    |
| 同姓同名／複数言語別名 | 誤統合を防ぎ、統合の取り消し後も根拠が失われない                    |
| 複数値の併存      | 兼業・複数住居を単純に旧値の矛盾として消さない                     |
| 仮定・予定・否定・引用 | 実現した事実へ無条件に昇格しない                            |
| 一つのソースの撤回   | 独立した別の根拠がある主張を必要以上に削除しない                    |
| schema の改訂  | 旧 predicate のデータを新 schema へ写像し、由来を保つ        |
| 二重送信・並行更新   | 同じイベントで二重に強化されず、lost update がない             |
| tenant と観測者 | 利用可能でない原資料に由来する要約も漏れない                      |
| 削除後の再構築     | cache / embedding / graph の再構築で削除済み情報が復活しない |
| 検索不能・知らない質問 | 誤答を作らず、根拠不足を示す                              |
| 日本語特有の表現    | 省略主語、敬称、姓・名、和暦、相対日付、全半角を扱える                 |
| 複数モダリティ     | OCR／音声転記の誤りと、元の画像領域・時間範囲を追える                |

## 指標

書き込みは assertion の precision/recall、実体同定の誤統合率、更新操作の適合率、出典 span の正しさ。検索は recall\@k と stale-fact rate、権限違反率、必要な根拠集合の回収率。回答は QA 正答、引用の支持、unknown の適切さ、棄却率と誤答率のトレードオフ。行動は downstream task success と負の転移を別に測る。

費用は一回の回答だけでなく、取り込み、統合、再埋め込み、削除、再試行を含める。LLM / embedding / rerank の呼び出し数、tokens、p50/p95、検索可能になるまでの遅延を保存する。書き込みで節約し検索で高い費用を払う方式と、その逆を比較できるようにする。

LLM judge は人手で層化サンプルを二重採点し、採点器変更への頑健性を確認する。原 LongMemEval は専用 rubric と人手一致の検討を含む。Ragas の faithfulness は与えた context への忠実性であり、その context が現実に正しいことまでは保証しない。[P-LME] [D-RAGAS]

## 関連ドキュメント

- [研究の系譜と各論文の位置づけ](papers.md)
- [検証仮説と実験の順序](design-directions.md)
- [全体設計と実装順序](../design/ontology-knowledge-system.md)
- [PageIndex の方式・制約・公開評価条件](systems/pageindex.md)

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

[D-PI-START]: https://docs.pageindex.ai/getting-started

[D-PI-FLASH]: https://pageindex.ai/blog/pageindex-flash

[D-PI-OSS-BENCH]: https://github.com/VectifyAI/PageIndex-OSS-Benchmark/blob/ad4c0b92970a6f4801f09ff2e647389e8f5874fa/README.md

[D-PI-FINANCE]: https://github.com/VectifyAI/Mafin2.5-FinanceBench/blob/1c890d5e0fd9929953d38282614555847727011d/README.md

[D-PI-FINANCE-EVAL]: https://github.com/VectifyAI/Mafin2.5-FinanceBench/blob/1c890d5e0fd9929953d38282614555847727011d/eval.py

[C-PI-LOCAL]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_api.py

[C-PI-STORE]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_store.py

[C-PI-TOOLS]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/agent_tools.py
