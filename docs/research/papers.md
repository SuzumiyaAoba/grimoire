# 研究の系譜と最近の方向

[調査トップ](README.md) / [資料台帳](sources.md)

年は原則として初稿年。査読済みと明記する場合は arXiv の publication comment または会議記録を確認した。掲載を確認できなかったものはプレプリントとして読む。書誌・版・確認の深さは[資料台帳](sources.md)を参照。

**2026-09-29追加:** [最先端研究への拡張](#最先端研究への拡張)では、中核実装が非公開の研究、部分公開の研究、実装前の理論提案も比較する。方式・著者の評価・限界と、コード/重み/データの公開状況を分けて読む。

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

## 最先端研究への拡張

**確認日: 2026-09-29。公開実装の有無を問わず、20研究を4分野で追加比較した。** 新しい論文だけでなく、既存資料で概要に留まっていた研究も含む。製品数の追加集計ではなく、仮説・手法・評価を比較する単位である。

中核が非公開のMOOSEDev/MOOSE、実行モデルを提示しないNEST、今回の範囲で公式実装を確認できなかった研究も採用した。コードが公開されている研究についても、重み・データ・実験ログがそろっているかは別に記録する。実装の公開は研究を読むための必須条件にしない。[P-F3-MOOSEDEV] [D-F3-MOOSEDEV] [P-F3-NEST]

「最先端」は2025–2026年の新しい研究方向まで探索する意味で、最良性能の認定ではない。プレプリント、採択情報のある論文、理論提案を区別し、著者の評価は追試していない。公開情報で説明できない非公開内部は推定しない。

今回広がった設計上の焦点は、**保存容量に加え、更新の規則、検索・提示の制御、利用時の有効性**である。公開実装を入手できなくても、論文で説明された仮説を既存設計の比較条件へ取り込める。以下では、著者の報告と、その報告から作る本調査の仮説を分ける。

### 比較する四つの層

| 対象 | 代表研究 | 変える対象 | 評価の焦点 |
|---|---|---|---|
| 構造化知識・理論 | MOOSEDev / NEST / EnSIMem | 何を信念・関係・証拠として表現するか | 集合の網羅、置換、原文への到達 |
| 外部記憶の制御 | MEMO / ReFind / Filesystem Memory / MemCon | いつ検索・整理し、どの証拠を渡すか | 回答・行動、検索費用、成長後の整理状態 |
| モデル内部・学習 | MIRAS / Nested Learning / Memory Layers / SMF / TTT / SDM | 内部の容量・更新則・時間尺度 | 学習・推論計算量、保持、干渉と忘却 |
| ライフサイクルの評価 | Fortunate Recall / STALE / Trust Gap / Revoked / TEPA / EAL / TRACE | その記憶を今も採用できるか | 失効、撤回後の行動、正当な利用の維持 |

外部ストアの更新とモデル重みの更新では、根拠を引用する方法も削除の意味も異なる。同じ「memory」という名称から、相互に置き換え可能とは判断しない。以下の設計への示唆は本調査の分析であり、著者がgrimoireの構成を検証したという意味ではない。

### 構造化知識・理論

型、関係、原資料との対応から記憶を設計する。

#### MOOSEDev / MOOSE

初稿: 2026-08-13 / 確認版: v1。arXiv書誌にNeSy 2026 Industry Track採択の記載あり。会議録の独立確認はしていない。

**方式:** 意思決定・制約・理由をOWL/SHACLの型付き記録にし、来歴と置換関係を辿って現在有効な知識を取得する。

**著者の評価:** CodeGraph由来835件の型付き記録で集合の網羅、否定、置換の質問を比較。回答器はGPT-5.x系列、judgeはGPT-5.4-mini。構造質問で改善を報告する一方、関連性検索は概ね同等と報告。 [P-F3-MOOSEDEV] [D-F3-MOOSEDEV]

**限界・未確認点:** 構造化captureの品質とagentのtool呼出に依存。大きい集合の列挙はtoken費用が増える。単一agent系列の事例で、数か月の理解維持は継続試験段階。 [P-F3-MOOSEDEV] [D-F3-MOOSEDEV]

**公開範囲:** 部分公開・中核非公開を明記。MOOSEDevはApache-2.0、MOOSE engineはclosed source。ソースビルドに非公開engineが必要。 [P-F3-MOOSEDEV] [D-F3-MOOSEDEV]

**設計への示唆（本調査の分析）:** 構造質問と曖昧な関連性検索を分け、置換関係・型・原資料参照の効果を個別に検証する比較候補。

#### NEST

初稿: 2026-07-07 / 確認版: v1。プレプリント。採択情報は今回の書誌で確認できず。

**方式:** 永続的な信念graphと容量制限付き作業記憶graphを分け、衝突・根拠付け・信念更新を共通のgraph表現で定義する。

**著者の評価:** 理論・表現枠の提案。ACT-RやSoar等との対応を示すが、実行比較やbenchmarkによる優位は報告していない。 [P-F3-NEST]

**限界・未確認点:** 6.2で学習algorithmは未指定、belief updateは抽象的、他体系との対応は実行simulationではないと明記。 [P-F3-NEST]

**公開範囲:** 理論提案・実行モデル未提示。6.3は計算的実装を今後の課題とする。論文・名称検索範囲で著者公式実装を確認できず。 [P-F3-NEST]

**設計への示唆（本調査の分析）:** 検索で得た作業上の仮説と確定した信念を分ける設計語彙になる。導入可能なmemory engineとして扱わない。

#### EnSIMem

初稿: 2026-09-23 / 確認版: v2（2026-09-27）。書誌にpreprintと記載。

**方式:** entity・type・propertyの索引をepisodeと原発話へ結び、質問を必要証拠に分解して検索する。回答は保存した原文根拠から生成する。

**著者の評価:** LoCoMo/LongMemEval。GPT-4.1-miniで構築/回答、GPT-4o-miniとQwen3-32Bのjudgeを別々に報告。固定subsetで構造索引とdense検索等を比較。 [P-F3-ENSIMEM] [D-F3-ENSIMEM]

**限界・未確認点:** 会話想起の評価で、技能実行・長期の意味統合は今後の課題。計画/検索のonline latencyが増える。比較表の既報値と自前実験、全体とablation subset、offline構築費用の除外を区別する。 [P-F3-ENSIMEM] [D-F3-ENSIMEM]

**公開範囲:** 著者公式公開repositoryの存在を確認。ファイル一覧の確認に留め、再現一式の完全性は未検証。 [P-F3-ENSIMEM] [D-F3-ENSIMEM]

**設計への示唆（本調査の分析）:** 型付き索引を原資料へ戻るアドレスとして使う発想を、現行EvidenceRef/SourceSpanの設計と比較できる。

### 外部記憶の検索・整理・提示

保存形式と、何をいつ取り出して使うかという制御を分ける。

#### MEMO

初稿: 2026-09-07 / 確認版: v1 (2026-09-07)。arXivプレプリント。コメント欄に8ページ・2図・Working in progressと記載。査読採録は確認できない。

**方式:** クエリと候補記憶から学習済み証拠抽出器が出典・文字span付きの証拠単位を作り、学習済みmemory managerが各単位をtext/image/dual/dropに割り当て、レイアウトも選ぶ。凍結readerによるオフライン結果でmanagerを学習し、決定的builderが共通予算下のworking memoryを構築する。

**著者の評価:** 著者は複数readerで、multi-hop QA・長期会話・ALFWorldを共通予算下で比較し、証拠の抽出と提示形式を組み合わせた方式が複数baselineを上回ると報告。アブレーションも実施。記載値は論文報告で、当方では追試していない。 [P-F3-MEMO]

**限界・未確認点:** 本文に専用Limitations節はない。v1の進行中研究であり、学習例は各ベンチマークの訓練側証拠annotationまたは決定的weak labelに依存する。主張は著者の4ベンチマーク・固定reader群の実験結果で、独立追試や未見タスクへの外部検証は本調査で確認していない。 [P-F3-MEMO]

**公開範囲:** 論文本文とarXivのCode/Data欄に著者コードのリンクを確認できず、完全一致タイトルのGitHub検索でも公式repositoryを特定できなかった。公開されていないとは断定しない。 [P-F3-MEMO]

**設計への示唆（本調査の分析）:** 外部メモリーの保存形式やgraph構造ではなく、検索後にどの証拠を残し、限られた入力予算でどう提示するかを学ぶ新しい読出し制御。原資料span・出典保持と圧縮／マルチモーダル表示の比較対象になる。

#### ReFind

初稿: 2026-08-13 / 確認版: v2 (2026-08-16)。arXivプレプリントv2。arXivコメント欄に採録先の記載を確認できない。

**方式:** 元の会話ログを変更せずturn単位で語彙index化し、反復keyword searchとsession-aware rank fusion、局所context展開、時間範囲の絞り込み、既読sessionの除外を組み合わせる。retrieval agentが証拠を集め、別stageのreaderが回答する。

**著者の評価:** 著者は会話記憶検索benchmarkで、同じLLMによる比較とLongMemEval評価を行い、生ログへの反復的な語彙検索が構造化memory・汎用agentic検索より概ね高い回答精度を示したと報告。baselineの一部は既報値。論文結果の要約で、当方では追試していない。 [P-F3-REFIND]

**限界・未確認点:** 対象は会話記録に対する正確な根拠検索・事実追跡で、意味圧縮や低遅延な常時readには別機構が必要と著者自身が位置づける。MABench比較では一部baseline値を既存論文から再利用し、全systemを同一controller/tool-call予算で再実行していない。1 query平均約5 LLM callsと検索制御コストがある。 [P-F3-REFIND]

**公開範囲:** arXiv本文・artifact欄に著者repositoryのURLを確認できず、完全一致タイトルのGitHub検索でも公式repositoryを特定できなかった。公開されていないとは断定しない。 [P-F3-REFIND]

**設計への示唆（本調査の分析）:** 整理済みメモリーを前提とせず、未加工の原記録から対話的な検索と時間・session情報で根拠を組み立てる。変換前保存と構造化memory構築の費用・回答品質を切り分ける比較候補。

#### Filesystem-Based Memory for LLM Agents

初稿: 2026-07-29 / 確認版: v1 (2026-07-29)。arXivプレプリントv1。arXivコメント欄に採録先の記載を確認できない。

**方式:** Markdown file treeを共有storeとし、管理agentが新着内容を追加・改訂・統合・分割・移動・削除し、search agentが引用付き回答を返す。実行agentの軌跡をskillsに蒸留する設定も統合する。agentのtool harness自体を比較因子として扱う。

**著者の評価:** 著者は会話QA・skill-reuse条件で、agentが整理した階層file storeをverbatim dump・chunk retrievalと比較。整理は大規模storeで検索コストを下げた一方、回答精度を一貫して上げないと報告。結果は限定された評価slice・horizonに基づき、当方では追試していない。 [P-F3-FILESYSTEM]

**限界・未確認点:** 著者は、測定したのは最長140 tasksと単一会話規模で月単位の長期蓄積ではないと明記。LoCoMoはheld-out会話1件、PersonaMem 32kは3会話に縮約するなど、各benchmarkの評価sliceが限定的。論文の結果はその測定horizon・model/tool条件に限定して読む必要がある。 [P-F3-FILESYSTEM]

**公開範囲:** 本文とarXivのCode/Data欄に著者コードのリンクを確認できず、完全一致タイトルのGitHub検索でも公式repositoryを特定できなかった。公開されていないとは断定しない。 [P-F3-FILESYSTEM]

**設計への示唆（本調査の分析）:** 日常のagent memoryに近いMarkdown filesystemを対象に、整理の質・検索効率・蓄積後のstore健全性を個別測定する。実装論文に限定しない研究比較のよい例で、製品機能比較で見えない運用horizonの問いを補う。

#### MemCon (Memory as a Controlled Process)

初稿: 2026-07-15 / 確認版: v1 (2026-07-15)。arXivプレプリントv1。本文に投稿先・採録先の記載を確認できない。

**方式:** 既存backendをMDPの操作先として包み、離散化したtask/memory状態からcontextual bandit + UCBがRetrieve、PlanInject、Re-Retrieve、Consolidate、Forget、NoOpとtop-k等の引数を選ぶ。成功/失敗のtask feedbackでオンライン更新し、事前学習や追加LLM callを使わないと説明。

**著者の評価:** 著者は複数benchmark・agent framework・LLM backboneで、固定policyやmemory baselineと比べ、学習controllerによるtask success向上とtoken使用減少を報告。component ablationも実施。数字は論文報告で、当方では再現していない。 [P-F3-MEMCON] [P-F3-MEMCON-REPO]

**限界・未確認点:** policy stateはtask type/phase/stuck状態など人手設計の離散featureに依存し、各backendのmaintenance hookがなければConsolidate/Forgetはno-op。汎用wrapperでも、バックエンドが提供しない履歴・削除保証を付与するものではない。独立追試は未実施。 [P-F3-MEMCON] [P-F3-MEMCON-REPO]

**公開範囲:** 著者論文に記載の公式GitHub repositoryをwebで開き、READMEの構成とmemory_mdp.py・policy.py・wrapper.pyの実ファイル表示を確認。公開状態とファイル存在の確認までで、コード監査・実行・再現確認はしていない。参照はmainブランチでcommit SHA未固定。 [P-F3-MEMCON] [P-F3-MEMCON-REPO]

**設計への示唆（本調査の分析）:** 保存形式を固定せず、タスク進捗・memory量・停滞に応じて何をいつ検索し、再検索・整理するかをオンラインで適応する。query-time検索の固定heuristicと書込・整理policyを比較する候補。

### モデル内部記憶・継続学習

内部の記憶容量、更新規則、学習する時間尺度を比較する。

名称に注意する。MIRASのMEMORAは系列モデルの変種で、外部メモリーの同名方式やTRACEの評価対象とは別に扱う。Memory LayersのMemory+とMemoryLLM系列のM+、認知表現のNESTとNested Learningも別研究である。

#### MIRAS / MONETA・YAAD・MEMORA

初稿: 2025-04-17 / 確認版: v1。ICLR 2026の公式ポスター・論文掲載を確認。

**方式:** 系列モデルをkey-value連想記憶として捉え、内部メモリ構造・記憶目的関数（attentional bias）・retention gate・更新algorithmの4軸で統一する。異なる損失や保持制御を使う深層メモリ変種を設計する枠組み。

**著者の評価:** 論文はFineWeb-Edu/C4による言語モデル学習、commonsense QA、RULERのneedle-in-a-haystackを報告。4,096 token学習や最大8K文脈の想起課題を含む。全て著者報告。 [P-F3-LRN-MIRAS] [P-F3-LRN-MIRAS-ICLR] [P-F3-LRN-MIRAS-GOOGLE]

**限界・未確認点:** 実験は系列モデリング、言語モデル、合成想起課題。個人の事実記憶・会話横断の訂正、根拠来歴、削除を評価しない。設計枠組みから実運用memory systemの保証は導けない。 [P-F3-LRN-MIRAS] [P-F3-LRN-MIRAS-ICLR] [P-F3-LRN-MIRAS-GOOGLE]

**公開範囲:** 論文、Google Research紹介、ICLRページで著者公式コードへのリンクを確認できず。実装が存在しないとは断定しない。 [P-F3-LRN-MIRAS] [P-F3-LRN-MIRAS-ICLR] [P-F3-LRN-MIRAS-GOOGLE]

**設計への示唆（本調査の分析）:** コードや重みの導入可否に依らず、記憶容量・類似度/損失・忘却・オンライン更新を分けて考える理論的な比較軸になる。

#### Nested Learning / HOPE

初稿: 2025-12-31 / 確認版: v1。NeurIPS 2025 Main Conferenceの公式会議録掲載を確認。arXiv初稿は会議開催後の2025-12-31。

**方式:** 学習器を異なる頻度・context flowを持つ多段の最適化問題として扱う。Adam等のoptimizerも勾配情報を圧縮する連想記憶と解釈し、自己更新型系列モデルと短期・長期を連続体として扱うContinuum Memory Systemを組み合わせHOPEを構成する。

**著者の評価:** 言語モデル、知識取り込み・few-shot、継続学習、長文脈推論を評価。本文はRULER、LongHealth、QASPER等を含む課題群を報告。小規模から中規模の研究設定による著者報告で、独立追試なし。 [P-F3-LRN-NEST] [P-F3-LRN-NEST-NEURIPS] [P-F3-LRN-NEST-GOOGLE]

**限界・未確認点:** 多段最適化による記憶の見方とHOPEは実証的提案だが、実験はモデル学習・benchmark課題。利用者の複数sessionをまたぐ事実台帳、出典保持、訂正・撤回を検証しない。 [P-F3-LRN-NEST] [P-F3-LRN-NEST-NEURIPS] [P-F3-LRN-NEST-GOOGLE]

**公開範囲:** 確認したNeurIPS・arXiv・Google Research公式資料で著者公式repositoryを特定できず。第三者のHOPE実装は検索結果にあるが、著者公式ではない。 [P-F3-LRN-NEST] [P-F3-LRN-NEST-NEURIPS] [P-F3-LRN-NEST-GOOGLE]

**設計への示唆（本調査の分析）:** 具体実装に依存しない、更新頻度・状態圧縮・optimizerを含む学習過程自体を記憶として見る枠組み。長期学習の理論比較に使える。

#### Memory Layers at Scale

初稿: 2024-12-12 / 確認版: v2 (2024-12-20)。ICML 2025、PMLR volume 267の公式会議録掲載を確認。

**方式:** FFNの一部を疎なkey-value lookupに置き換え、top-kで活性化する学習可能な記憶層を追加。Memory+は射影と入力依存gateを加え、少ない追加FLOPsで大きなパラメータ容量を持たせる。

**著者の評価:** Llama系134M–1.3Bの学習と8B基盤を含み、最大128B memory parameters、1T training tokensを報告。知識QA、commonsense、MMLU、HumanEval等を評価し、計算量・parameter-matchedなdense/MoEと比較。数値は著者報告。 [P-F3-LRN-MEMLAYERS] [P-F3-LRN-MEMLAYERS-ICML] [P-F3-LRN-MEMLAYERS-REPO]

**限界・未確認点:** 事前学習で蓄える容量であり、推論中にユーザーごとの記憶を編集する仕組みではない。小規模基盤での学習不安定性や正規化の必要を報告。出典・訂正・忘却を評価していない。 [P-F3-LRN-MEMLAYERS] [P-F3-LRN-MEMLAYERS-ICML] [P-F3-LRN-MEMLAYERS-REPO]

**公開範囲:** Meta FAIR公式repositoryにreference implementationを確認。 [P-F3-LRN-MEMLAYERS] [P-F3-LRN-MEMLAYERS-ICML] [P-F3-LRN-MEMLAYERS-REPO]

**設計への示唆（本調査の分析）:** 学習可能な疎なlookupの容量/FLOPs trade-offを比較できる。Memory LayersのMemory+とMemoryLLMのM+は別系統であり、外部記憶との比較では事前学習した知識とセッション中の状態を区別する。

#### Continual Learning via Sparse Memory Finetuning (SMF)

初稿: 2025-10-16 / 確認版: v1。arXiv preprint。公開審査PDFにはICLR 2026審査中表記がある。2026-09-29時点で公式採択一覧に題名を確認できず、採択扱いしない。

**方式:** Memory Layersのactivation slotを用い、新しいbatchでpretraining背景より相対的に強く活性化したtop-t slotだけを更新する。更新対象を疎にして既存能力との干渉を抑える。

**著者の評価:** 1.3B基盤にmemory layerを置き、TriviaQA/SimpleQAの新知識学習をFull FT・LoRAと比較。NaturalQuestions F1の低下をそれぞれ89%、71%、11%と報告し、新知識獲得量を揃えた条件を示す。評価は2 QA task。 [P-F3-LRN-SMF] [P-F3-LRN-SMF-OPENREVIEW] [P-F3-LRN-SMF-AUTHOR] [P-F3-LRN-SMF-ICLR] [P-F3-LRN-SMF-COMMUNITY]

**限界・未確認点:** 短い知識注入と質問応答の設定であり、連続会話の流入、事実の時系列変化、訂正・根拠追跡を検証しない。報告数値は著者評価で、独立追試なし。 [P-F3-LRN-SMF] [P-F3-LRN-SMF-OPENREVIEW] [P-F3-LRN-SMF-AUTHOR] [P-F3-LRN-SMF-ICLR] [P-F3-LRN-SMF-COMMUNITY]

**公開範囲:** 論文・著者ページで公式code linkを確認できず。第三者repositoryのREADMEに実装を確認したが著者公式とは確認せず、実装品質も未監査。 [P-F3-LRN-SMF] [P-F3-LRN-SMF-OPENREVIEW] [P-F3-LRN-SMF-AUTHOR] [P-F3-LRN-SMF-ICLR] [P-F3-LRN-SMF-COMMUNITY]

**設計への示唆（本調査の分析）:** 一般的なfine-tuneの忘却を、疎な連想記憶slotへの選択更新で抑える学習方法。モデル内部記憶が長期更新に使える条件を検討する候補。

#### End-to-End Test-Time Training for Long Context (TTT-E2E)

初稿: 2025-12-29 / 確認版: v2 (2025-12-31)。arXiv preprint。査読採択を確認できず。

**方式:** sliding-window Transformerを各sequenceのnext-token予測で推論時にも更新し、読んだcontextをfast weightsへ圧縮する。学習時のmeta-learningでtest-time update後の損失を直接最適化する。

**著者の評価:** 3Bモデルを164B tokenで学習。long-context言語モデル評価を最大128Kまで実施し、著者は128Kでfull-attentionより2.7倍速い定常decode latencyを報告。DCLM事前学習とBooks長文延長を含む。 [P-F3-LRN-E2E] [P-F3-LRN-E2E-REPO]

**限界・未確認点:** 主眼は文脈長に応じたLM性能・計算量で、単一sequence内の学習状態を扱う。session横断のユーザー事実、選択的更新、根拠source、訂正/rollbackを検証していない。 [P-F3-LRN-E2E] [P-F3-LRN-E2E-REPO]

**公開範囲:** 著者公式JAX repositoryを公開。 [P-F3-LRN-E2E] [P-F3-LRN-E2E-REPO]

**設計への示唆（本調査の分析）:** 通常の外部memory layerを足さず、test-time learningでcontextを重みに圧縮する最先端例。長期記憶の「逐語保持」と「経験圧縮」の区別を考える材料。

#### Sparse Delta Memory (SDM)

初稿: 2026-07-08 / 確認版: v1。arXiv v1 preprint。査読採択情報は今回確認できず。

**方式:** Gated DeltaNet型の系列モデルにproduct-keyでアドレスする大規模疎メモリ表を加え、読み書きとdelta updateを疎にする。初期状態M0に事前学習知識を持たせ、sequenceを読むごとの状態更新と分ける。

**著者の評価:** isoFLOPs・同一parameter budget（実行時memory stateを除外）の条件で線形RNNの状態容量を約1000倍にできると著者報告。1.4B/8B、最終8Bで1T超tokensの学習、128K長文評価、RULER 4K–131Kや長文code課題を報告。 [P-F3-LRN-SDM] [P-F3-LRN-SDM-REPO]

**限界・未確認点:** 論文は状態memory footprintが無視できず、model parameter規模に近づき得ることと、8B超への拡大にはkernel改善が必要とする。合成長文/RULERやLM評価では個人記憶の出典・訂正・消去を検証しない。 [P-F3-LRN-SDM] [P-F3-LRN-SDM-REPO]

**公開範囲:** Meta FAIRの公式reference implementationとTriton/CUDA kernelsを公開。 [P-F3-LRN-SDM] [P-F3-LRN-SDM-REPO]

**設計への示唆（本調査の分析）:** 2026年の内部疎メモリ研究。parameter数/FLOPsの効率だけでなく、実行時状態のサイズを独立に計測する必要性を示す。

### 訂正・失効・撤回・共有状態の評価

過去に正しかった記憶が、今の回答・行動に使えるかを調べる。

#### Fortunate Recall

初稿: 2026-09-09 / 確認版: v1。arXivプレプリント。書誌欄は査読中と明記

**方式:** 個人的事実を行動型10+1分類へ割り当て、型別の減衰、slot置換、有効時刻、検索方針を抽出済みメタデータ上で決定的に適用する。

**著者の評価:** LifecycleBench 516問とLongMemEval-S 500問を評価。汎用的な有効期間・置換操作だけにした対照でも正答差は明確でなく、型分類は主に回答校正に寄与したと報告。 [P-FR] [A-F3-FR]

**限界・未確認点:** 人手による回答検証がなくLLM judge依存。著者作成ベンチマークの同時設計を含み、10+1分類が最適・必須とは示していない。査読中。 [P-FR] [A-F3-FR]

**公開範囲:** 論文は公開済みと明記し、Zenodoにcodebase ZIPを掲載。 [P-FR] [A-F3-FR]

**設計への示唆（本調査の分析）:** 鮮度と置換を事実型に分ける構成例だが、撤回・根拠依存の派生物再評価は別に設計・評価する必要がある。

#### STALE: Can LLM Agents Know When Their Memories Are No Longer Valid?

初稿: 2026-05-07 / 確認版: v1。arXiv v1。確認した書誌欄に会議・査読状態の記載なし

**方式:** 新観察が旧属性を直接または関連属性経由で失効させる暗黙矛盾を定義し、状態解決・古い前提への抵抗・暗黙の行動適応の三方向で測る。CUPMemは書込時状態統合と伝播検索の試作。

**著者の評価:** 専門家確認済み400場面・1,200質問、Type I同一属性とType II依存属性の失効を比較。長文脈は最大150K tokens。最高報告モデルの全体正答率は55.2%。 [P-F3-STALE] [P-F3-STALE-HTML] [A-F3-STALE-CODE] [A-F3-STALE-DATA]

**限界・未確認点:** 一場面一組の管理された矛盾で、反復更新・段階的変化・複数属性の複雑な依存を十分に表さない。生成対話とLLM judgeに依存し、実利用分布とは差がある。 [P-F3-STALE] [P-F3-STALE-HTML] [A-F3-STALE-CODE] [A-F3-STALE-DATA]

**公開範囲:** 著者GitHubでベンチ生成・評価・CUPMem実装を公開。 [P-F3-STALE] [P-F3-STALE-HTML] [A-F3-STALE-CODE] [A-F3-STALE-DATA]

**設計への示唆（本調査の分析）:** 訂正は明示語の一致だけでは発見できず、関連属性の変更から旧推論・助言を失効させる試験が必要。

#### The Memory Trust Gap: Capability-Dependent Failures in Persistent-Memory Agents

初稿: 2026-09-01 / 確認版: v1。プレプリント。著者はNeurIPS 2026 workshopで審査中と書誌欄に記載

**方式:** 記憶が必要なBenefit群と、正解を持つ権威ツールが常にあるSafety群を分け、古い値を信じる率とno-memory比の正答差を別指標にする。

**著者の評価:** 固定300場面でQwen3 0.6/1.7/4/8Bを評価。記憶に沿って行動する率とno-memory比の正答差を分けて報告し、Llama系列やRGB・MisBenchでも一部再検証。 [P-F3-TRUST] [P-F3-TRUST-HTML]

**限界・未確認点:** 主系列は単一モデル族。閉集合の行動採点で、実際の書込・更新・検索一連の処理は測らない。Memory固有でない古い証拠への過信も示す。 [P-F3-TRUST] [P-F3-TRUST-HTML]

**公開範囲:** 確認した論文本文・arXiv書誌に公開先の明記なし。外部での公開有無は未判定。 [P-F3-TRUST] [P-F3-TRUST-HTML]

**設計への示唆（本調査の分析）:** 正しい記憶を保持する利益と、古い記憶が現在の根拠を上書きする害を別々に比較する基準を与える。

#### Revoked but Still Authoritative: An Empirical Study of Revocation Enforcement in Agent-Memory Systems

初稿: 2026-09-08 / 確認版: v1。arXiv v1。確認した書誌欄に会議・査読状態の記載なし。コード公開先を明記

**方式:** 撤回済み方針と代替方針を記憶系へ投入し、検索が旧方針を返すか、行動器が従うかを分けて測定。検索前段のguardは撤回記録と置換候補を照合して抑止。

**著者の評価:** 5記憶系、9方針シナリオ、9モデル、6防御条件を比較。検査条件ではretrieval filterがunsafe actionを0/1,620まで抑えた一方、単独prompt hardeningは残余行動を止めきれなかった。 [P-F3-REVOKED] [P-F3-REVOKED-HTML] [A-F3-REVOKED-CODE]

**限界・未確認点:** 結論は試験したadapter・設定と合成方針シナリオの範囲に限る。単発検索だけのfilter保証は、判断をjournalへ書き戻すと新しい現行記録に迂回され得る。 [P-F3-REVOKED] [P-F3-REVOKED-HTML] [A-F3-REVOKED-CODE]

**公開範囲:** 論文にGitHub公開を明記し、guardと複数実験のコードを掲載。 [P-F3-REVOKED] [P-F3-REVOKED-HTML] [A-F3-REVOKED-CODE]

**設計への示唆（本調査の分析）:** 撤回状態の保存では足りず、検索時の適格性と、撤回済み根拠から作った要約・決定の再流入も監査すべきと示す。

#### TEPA: Revoking Stale Memories for Conflict-Robust Language Agents

初稿: 2026-08-07 / 確認版: v2（2026-08-10改訂）。arXiv書誌でpreprintと明記

**方式:** 観測をkey付きprecedentとして保持し、有効状態の項目と同じkeyで新根拠が衝突すれば旧項目を撤回し、通常検索から除いて監査用履歴には残す。検証済み昇格の変種も評価。

**著者の評価:** 50 seedの隠れた状態変化・実ファイル実行・嗜好更新とMemoryAgentBenchを評価。完全反転時の統制課題でTEPAは成功率0.95、append-onlyとlast-write-winsは0.21。 [P-F3-TEPA] [P-F3-TEPA-HTML]

**限界・未確認点:** 同じ事実を結ぶkey抽出が前提。多段推論・非常に長い文脈では検索連鎖とcontext選択が別のボトルネックになる。 [P-F3-TEPA] [P-F3-TEPA-HTML]

**公開範囲:** 確認した論文書誌・本文に配布先の記載なし。有無は未判定。 [P-F3-TEPA] [P-F3-TEPA-HTML]

**設計への示唆（本調査の分析）:** 旧事実の保持と現行検索対象からの撤回を分け、必要なら監査・再昇格できる具体モデルを提示。

#### Agent Memory Is a Surface for Endogenous Authorization Laundering

初稿: 2026-09-01 / 確認版: v1。arXiv v1。確認した書誌欄に会議・査読状態の記載なし

**方式:** 正規の組織履歴からwriterが記憶を作り、後段executorがその記憶だけで行動する。外部攻撃なしで更新時に権限境界が薄れるendogenous authorization launderingを測る。

**著者の評価:** 調達・サイバーセキュリティ・金融の3領域、5 writerと2 executorを評価。typed incremental更新では不許可依頼の最大50.2%に誤権限が生じ、その記憶があるとexecutorの98.6%が行動。 [P-F3-EAL] [P-F3-EAL-HTML] [A-F3-EAL-CODE]

**限界・未確認点:** 履歴は現実的な構造を残しつつ決定的判定のため明示的なイベントと模擬toolを使う。実運用頻度を推定しない。保護策は不正行動を減らす一方、正当操作も拒否。 [P-F3-EAL] [P-F3-EAL-HTML] [A-F3-EAL-CODE]

**公開範囲:** 著者のEAL-Bench公開repoに評価コード・領域定義・結果資料あり。 [P-F3-EAL] [P-F3-EAL-HTML] [A-F3-EAL-CODE]

**設計への示唆（本調査の分析）:** 知識内容の出典だけでなく、その出典が与える権限の主体・対象・範囲・期限・撤回を更新後も束縛する必要を示す。

#### TRACE: Governing Memory Validity in Evolving Multi-Agent Systems

初稿: 2026-09-27 / 確認版: v1。arXiv v1。9月29日に確認した書誌欄は掲載先を記載していない

**方式:** 復帰時に離脱時点のsnapshotを共有状態の更新と照合し、時間有効性・出典・適用範囲・役割の未完了事項を検証。条件を満たすbounded Return Viewだけを再投入。

**著者の評価:** 3 actor modelsで5-agentの復帰場面を評価し、Memora・STALE Type II・派生ManBench-Returnを使用。STALE Type IIで比較policyより改善する一方、tokenは約2.3倍。書込時統合pipelineには精度で及ばず、同pipelineはTRACEの約3.99倍のtokenを使うと著者報告。 [P-F3-TRACE] [P-F3-TRACE-HTML] [A-F3-TRACE-CODE]

**限界・未確認点:** 長い離脱で成績が低下し、暗黙依存の見落とし・過剰な棄却はverifierの範囲と校正に左右される。ManBench設定は派生評価で、報告token数は遅延・再試行費用を示さない。 [P-F3-TRACE] [P-F3-TRACE-HTML] [A-F3-TRACE-CODE]

**公開範囲:** arXiv本文と著者GitHubで公開を明記。MITライセンス。 [P-F3-TRACE] [P-F3-TRACE-HTML] [A-F3-TRACE-CODE]

**設計への示唆（本調査の分析）:** 検索できる・原文に忠実というだけでは現行行動への採用資格を示せず、再利用時点で共有状態に対する有効性を検査する案になる。

### 公開状態から何が分かるか

MOOSEDevはMCP・ontology等の周辺を公開する一方、MOOSE engineは非公開と明記する。NESTは学習アルゴリズムと実行比較を未提示と明記する。MEMOやReFind等は今回の確認範囲で公式実装を特定できなかった状態であり、著者が非公開と宣言した状態と区別する。コード・重み・データの個別の根拠URLと確認範囲は[研究台帳](evidence/frontier-research.json)に保存した。[D-F3-MOOSEDEV] [P-F3-NEST] [P-F3-MEMO] [P-F3-REFIND]

本調査はリポジトリのリンクやREADMEの存在を確認した範囲を、完全な学習・評価環境の公開とは呼ばない。公開予定と書かれた成果物も、配布物を確認した項目と分ける。理論の新しい視点、手法の実証、実装の再利用可能性は独立した判断になる。

### 採用した設計上の問い

構造質問ではSQLやgraphで必要集合を列挙する比較を加える。学習型memoryでは、既知の評価質問に有利な圧縮と、将来の未知課題でも根拠を保持する能力を分ける。内部記憶では、容量や言語モデルの評価値から原資料の監査・局所訂正・削除能力を推定しない。

さらに、訂正された事実を検索から外すこと、原資料から派生した要約を再評価すること、行動直前に現行の根拠と権限を検査することを別々に測る。正しい記憶を使う利益も残して評価し、記憶を全部無視して不適切な利用だけを減らす方式を改善と見なさない。これは今回の論文群から得た研究仮説であり、[H7–H10の実験案](design-directions.md#最先端研究から追加する仮説)と[評価方法](evaluation.md#実装が公開されていない研究の評価)に具体化した。

### 保留と調査の境界

WorldDBは、型ごとの書込時処理と再帰的な知識表現を設計案として補足する。ただし本文§7.1はLongMemEval-sと呼びつつ`longmemeval_oracle`を指定し、要旨と§7.4でgraphの寄与値も一致しない。条件確認なしに性能の優位を採用しない。実装未発見が保留理由ではなく、報告条件に未解決点があるためである。[P-F3-WORLDDB]

隣接候補の名称・URL・確認範囲・保留理由は[研究台帳](evidence/frontier-research.json)に残した。複数エージェントのrouting、長期業務状態、忘却の反実仮想評価などは次の探索先となる。保留は方式の否定ではない。検索全件を保存したsystematic reviewや、世界の非公開研究を網羅した一覧ではない。

今回の終了条件は、選定した4分野の研究について一次資料の提案・評価または理論上の位置づけ・限界を確認し、公開範囲と設計への接続を記録すること。製品の導入、モデルの学習、研究コードやAPIの実行、ベンチマークの追試は行っていない。

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

[P-F3-MOOSEDEV]: https://arxiv.org/html/2608.13662v1

[D-F3-MOOSEDEV]: https://github.com/Trivyn/moosedev

[P-F3-NEST]: https://arxiv.org/html/2607.06055v1

[P-F3-ENSIMEM]: https://arxiv.org/html/2609.27279v2

[D-F3-ENSIMEM]: https://github.com/RamonMeng/EnSIMem

[P-F3-WORLDDB]: https://arxiv.org/html/2604.18478v1

[P-F3-FILESYSTEM]: https://arxiv.org/html/2607.26637v1

[P-F3-REFIND]: https://arxiv.org/html/2608.12888v2

[P-F3-MEMO]: https://arxiv.org/html/2609.07471v1

[P-F3-MEMCON]: https://arxiv.org/html/2607.13591v1

[P-F3-MEMCON-REPO]: https://github.com/ericjiang18/MemCon

[P-F3-LRN-MIRAS]: https://arxiv.org/html/2504.13173v1

[P-F3-LRN-MIRAS-ICLR]: https://iclr.cc/virtual/2026/poster/10008141

[P-F3-LRN-MIRAS-GOOGLE]: https://research.google/blog/titans-miras-helping-ai-have-long-term-memory/

[P-F3-LRN-NEST]: https://arxiv.org/html/2512.24695v1

[P-F3-LRN-NEST-NEURIPS]: https://proceedings.neurips.cc/paper_files/paper/2025/hash/4309616aaed8e848009bc4a7ef73b493-Abstract-Conference.html

[P-F3-LRN-NEST-GOOGLE]: https://research.google/blog/introducing-nested-learning-a-new-ml-paradigm-for-continual-learning/

[P-F3-LRN-MEMLAYERS]: https://arxiv.org/html/2412.09764v2

[P-F3-LRN-MEMLAYERS-ICML]: https://proceedings.mlr.press/v267/berges25a.html

[P-F3-LRN-MEMLAYERS-REPO]: https://github.com/facebookresearch/memory

[P-F3-LRN-SMF]: https://arxiv.org/html/2510.15103v1

[P-F3-LRN-SMF-OPENREVIEW]: https://openreview.net/pdf?id=LGo7U1m24L

[P-F3-LRN-SMF-AUTHOR]: https://jessylin.com/

[P-F3-LRN-SMF-ICLR]: https://iclr.cc/virtual/2026/papers.html

[P-F3-LRN-SMF-COMMUNITY]: https://github.com/dtunai/continual_learning_via_sparse_memory_finetuning

[P-F3-LRN-E2E]: https://arxiv.org/html/2512.23675v2

[P-F3-LRN-E2E-REPO]: https://github.com/test-time-training/e2e

[P-F3-LRN-SDM]: https://arxiv.org/html/2607.07386v1

[P-F3-LRN-SDM-REPO]: https://github.com/facebookresearch/sparse-delta-memory

[A-F3-FR]: https://zenodo.org/records/20067778

[P-F3-STALE]: https://arxiv.org/abs/2605.06527v1

[P-F3-STALE-HTML]: https://arxiv.org/html/2605.06527v1

[A-F3-STALE-CODE]: https://github.com/icedreamc/STALE

[A-F3-STALE-DATA]: https://huggingface.co/datasets/STALEproj/STALE

[P-F3-TRUST]: https://arxiv.org/abs/2609.01852v1

[P-F3-TRUST-HTML]: https://arxiv.org/html/2609.01852v1

[P-F3-REVOKED]: https://arxiv.org/abs/2609.08258v1

[P-F3-REVOKED-HTML]: https://arxiv.org/html/2609.08258v1

[A-F3-REVOKED-CODE]: https://github.com/VulcanLab/Memory-Rebirth-Attack

[P-F3-TEPA]: https://arxiv.org/abs/2608.07429v2

[P-F3-TEPA-HTML]: https://arxiv.org/html/2608.07429v2

[P-F3-EAL]: https://arxiv.org/abs/2609.01836v1

[P-F3-EAL-HTML]: https://arxiv.org/html/2609.01836v1

[A-F3-EAL-CODE]: https://github.com/tommasocerruti/eal-bench

[P-F3-TRACE]: https://arxiv.org/abs/2609.33517v1

[P-F3-TRACE-HTML]: https://arxiv.org/html/2609.33517v1

[A-F3-TRACE-CODE]: https://github.com/xiong-wenjun/TRACE
