# 研究の系譜と最近の方向

[調査トップ](README.md) / [資料台帳](sources.md)

年は原則として初稿年。査読済みと明記する場合は arXiv の publication comment または会議記録を確認した。掲載を確認できなかったものはプレプリントとして読む。書誌・版・確認の深さは[資料台帳](sources.md)を参照。

## 研究の流れ

| 時期 | 研究 | 中心となるアイデア | 今回の設計に残す問い |
|---|---|---|---|
| 2020 | RAG | パラメータ知識と取得する外部文書を組み合わせる | 出典・更新を外側で扱えるが、更新方針そのものは別。[P-RAG] |
| 2023 | Generative Agents | 観察、重要度、反省、計画を連携 | believable な行動と事実正確性は別の評価。[P-GEN] |
| 2023 | MemoryBank | 忘却・強化と人物理解 | 保持優先度の減衰を真偽の変化と分ける。[P-MBANK] |
| 2023 | Reflexion / Voyager | 言語的な経験と再利用する skill | 成功条件・失敗条件を持った手続きの管理。[P-REFLEX] [P-VOYAGER] |
| 2023 | MemGPT / CoALA | 文脈階層の制御／認知アーキテクチャの整理 | 記憶を扱う制御器と保存形式を区別。[P-MGPT] [P-COALA] |
| 2024 | RAPTOR / GraphRAG | 木・コミュニティによる複数抽象度の検索 | 上位要約の更新と来歴。[P-RAPTOR] [P-MSGRAPH] |
| 2024–2025 | HippoRAG / HippoRAG 2 | graph と PPR、passage と recognition の統合 | associative な検索と時間的正しさを両立できるか。[P-HIPPO] [P-HIPPO2] |
| 2024 | LightRAG / KAG | 二層検索／schema と論理形式の利用 | 増分更新と domain semantics をどう接続するか。[P-LIGHT] [P-KAG] |
| 2025 | Zep / Mem0 | 時間付き関係／選別した事実の更新 | 新情報が候補集合にない既存矛盾をどう発見するか。[P-ZEP] [P-M0] |
| 2025 | A-MEM | リンクとノートの自動的な再編 | 書き戻しによる意味のずれをどう測るか。NeurIPS 2025 と書誌確認。[P-AMEM] |
| 2025 | MemoryOS / MemOS | 階層 storage／異種メモリーの統合 | 似た名前の別システム。記憶の昇格条件が重要。[P-MEMORYOS] [P-MEMOS] |
| 2025 | MIRIX | 六種のメモリーと複数モダリティ | スクリーン等の原資料と抽出内容の対応。[P-MIRIX] |
| 2025–2026 | Memory-R1 / MemRL / ACE | 更新方針の学習、utility による検索、経験の増分編集 | 学習対象と評価の独立性、負の転移。[P-MR1] [P-MEMRL] [P-ACE] |
| 2026 | SimpleMem / EverMemOS | 意味圧縮／episode→scene の統合 | 圧縮で残すべき例外・時刻・根拠。[P-SIMPLE] [P-EVER] |

## 更新方針・利用方針を学ぶ

**Memory-R1（初稿 2025-08）。** Memory Manager の記憶操作と Answer Agent の利用を、PPO / GRPO で改善する。報酬を最終回答から得るため、どの入力を長期保存すべきかを固定 heuristic から学習対象へ移す。ただし、ベンチマークの回答に有利な記憶と、将来別の目的で必要になる根拠付き知識は一致しない場合がある。評価用の質問・答えが管理方針の学習に漏れない分割が必要。[P-MR1]

**MemRL（2026-01）。** LLM の重みは固定し、Intent–Experience–Utility の形で経験を保存する。意味的に近い候補を選んだ後、環境のフィードバックで更新する Q-value により有用な経験を選ぶ。真偽の confidence とタスクでの効用を分けるよい実例。報酬が曖昧な一般対話では、utility の定義と偏りが課題になる。[P-MEMRL]

**ACE（初稿 2025-10、ICLR 2026）。** Generator、Reflector、Curator に相当する役割で、経験を playbook の差分として蓄積・整理する。全体を何度も短く書き直すことによる context collapse を問題にしている。今回への示唆は、過去の有益な条件・例外を保つ増分編集。対話の事実訂正と同じ目的関数ではない。[P-ACE]

**Memento-Skills（2026-03、技術報告）。** Markdown に技能を外部化し、read/write の反省ループで再利用・改訂する。手続き知識の有力な方向。コード・実行権限・環境依存を含む skill は、事実の保管と異なる採用条件を必要とする。[P-MSKILLS]

## 時間とオントロジーの最近の研究

**Fortunate Recall（2026-09-09、査読中プレプリント）。** 個人的事実を振る舞いの型へ分類し、減衰、slot 単位の置換、有効期間、検索方針へ接続する。今回の「ontology のような外部知識」と「更新」を直接つなぐ研究である。[P-FR]

この論文では、型体系を除いて汎用的な失効・置換・撤回だけを残した比較について、正確性の差は統計的に明確でないと報告する。一方で hallucination に関する校正面の効果を主張する。「10+1 分類を入れるだけで正答率が上がる」と一般化しない。Memory-R1 の比較構成には pre-RL と記載されるものがあり、学習済み方式全体の優劣には使えない。自作 benchmark、retrieval pass と最終正答の乖離、撤回した分析値への言及もあり、実験条件の保存が不可欠である。[P-FR]

**VikingMem（2026-05）。** event を中心に情報を取り込み、それが entity の状態を更新する memory base を提案する。topic ごとの時系列、時間を加味した retrieval、古い情報の圧縮を扱う。OpenViking は論文の能力の一部を公開すると説明しており、論文全体と公開実装の対応を追う必要がある。[P-VIKING]

**オントロジー学習。** AutoSchemaKG はデータから schema を誘導し、LLMs4OL は型・分類・関係を独立に評価する。生成した schema をそのまま採用する方法と、既存語彙への grounding を行う方法を比較する必要がある。[P-AUTOSCHEMA] [P-LLMOL] [P-LLMOL26]

## マルチモーダルとモデル内部の研究

MIRIX は画像を含む経験と複数の記憶型を扱う。テキストへ変換した後だけを見ると、OCR の誤り、画面内の対象位置、音声の話者・時刻を失う。原資料の範囲を参照する仕組みが必要になる。[P-MIRIX]

Titans と Engram は、ニューラルメモリーや条件付き lookup を計算構造に組み込む研究。外付けの knowledge store とは設計の層が違うため、同じ「長期メモリー性能」の順位表に置かない。[P-TITANS] [P-ENGRAM]

## 信頼性を扱う研究

AgentPoison は、外部記憶へ混入した情報が将来のツール使用・判断を変え得ることを示す。MemGate は、query と記憶の意味的類似だけでなく、注入の適切性を判定する層を提案する。どちらも、保存された記憶を高信頼の指示として再利用してしまう設計への検討材料となる。[P-POISON] [P-MEMGATE]

現時点の実装から、新規性を主張しやすい空白が即座に決まるわけではない。次の候補は、根拠撤回の正確な伝播、時間と視点を考慮した同一性、schema 変更時の再評価、知識の正しさと経験の効用の分離であり、[設計・実験案](design-directions.md)で検証仮説に落とす。

## 関連ドキュメント

- [既存システムの比較](systems/README.md)
- [評価方法と報告値の読み方](evaluation.md)
- [設計・実験案](design-directions.md)

[P-RAG]: https://arxiv.org/abs/2005.11401v4
[P-GEN]: https://arxiv.org/abs/2304.03442v2
[P-MBANK]: https://arxiv.org/abs/2305.10250v3
[P-REFLEX]: https://arxiv.org/abs/2303.11366v4
[P-VOYAGER]: https://arxiv.org/abs/2305.16291v2
[P-MGPT]: https://arxiv.org/abs/2310.08560v2
[P-COALA]: https://arxiv.org/abs/2309.02427v3
[P-RAPTOR]: https://arxiv.org/abs/2401.18059v1
[P-MSGRAPH]: https://arxiv.org/abs/2404.16130v2
[P-HIPPO]: https://arxiv.org/abs/2405.14831v3
[P-HIPPO2]: https://arxiv.org/abs/2502.14802v2
[P-LIGHT]: https://arxiv.org/abs/2410.05779v3
[P-KAG]: https://arxiv.org/abs/2409.13731v3
[P-ZEP]: https://arxiv.org/abs/2501.13956v1
[P-M0]: https://arxiv.org/abs/2504.19413v1
[P-AMEM]: https://arxiv.org/abs/2502.12110v11
[P-MEMORYOS]: https://arxiv.org/abs/2506.06326v1
[P-MEMOS]: https://arxiv.org/abs/2505.22101v1
[P-MIRIX]: https://arxiv.org/abs/2507.07957v1
[P-MR1]: https://arxiv.org/abs/2508.19828v5
[P-MEMRL]: https://arxiv.org/abs/2601.03192v2
[P-ACE]: https://arxiv.org/abs/2510.04618v3
[P-SIMPLE]: https://arxiv.org/abs/2601.02553v3
[P-EVER]: https://arxiv.org/abs/2601.02163v2
[P-MSKILLS]: https://arxiv.org/abs/2603.18743v1
[P-FR]: https://arxiv.org/abs/2609.10413v1
[P-VIKING]: https://arxiv.org/abs/2605.29640v3
[P-AUTOSCHEMA]: https://arxiv.org/abs/2505.23628v3
[P-LLMOL]: https://arxiv.org/abs/2307.16648v2
[P-LLMOL26]: https://arxiv.org/abs/2608.27101v2
[P-TITANS]: https://arxiv.org/abs/2501.00663v1
[P-ENGRAM]: https://arxiv.org/abs/2601.07372v2
[P-POISON]: https://arxiv.org/abs/2407.12784v1
[P-MEMGATE]: https://arxiv.org/abs/2606.06054v1
