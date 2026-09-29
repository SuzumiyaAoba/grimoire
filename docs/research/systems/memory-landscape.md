# 長期メモリーとエージェント経験の関連システム

[調査トップ](../README.md) / [システム比較](README.md) / [拡張調査](extended-landscape.md)

確認日: 2026-09-29。既存の追加候補ページで概要に留まっていた記憶系を深め、新規の長期メモリー／手順記憶候補を一次資料から比較する。対象は OpenViking、MemoryOS、ReMe の固定コミットで選択ファイルを静的確認し、他候補は公式文書、論文、README の範囲を記録した。インストール、製品 API、ベンチマークは実行していない。

## 調査の要点

**システムを見るときの要点**

1. `INFERRED` **記憶の役割は複数ある** — 会話履歴・実行状態、人物やプロジェクトの profile、出来事の episode、再利用する手順・skill、原資料を保存する resource は、同じ「memory」名でも別の要件を持つ。万能な単一メモリーと仮定せず、必要な役割から選ぶ。
2. `INFERRED` **更新責任の違いが大きい** — 文書を人が編集する方式、ルールで時系列にまとめる方式、LLM が情報を抽出・統合する方式、タスク経験を手順として学習する方式では、誤りの訂正、根拠への追跡、削除後の派生記憶の処理が異なる。
3. `INFERRED` **Ontologyとの接点は意味型と制約** — reviewed systems の多くは profile・event・tag・link 等の独自 schema を持つが、公開資料だけでは外部 ontology や推論規則への準拠を確認できない。人物・組織・期間・出典を相互運用したい場合、ontology を必要な境界に別途適用する。
4. `INFERRED` **論文スコアは実装保証ではない** — 論文の評価条件と現行リポジトリ、ホスト API の機能を分ける。本文記載の数値は著者報告として扱い、この調査で再現した結果ではない。

既存の浅い範囲は[追加候補の概要](additional-systems.md)、事実抽出と profile を中心にした方式は[事実メモリー](fact-memory.md)、agent・文書・実行基盤の境界は[エージェントメモリー](agent-memory.md)を参照。ここでは重複説明を避け、差分と今回確認した範囲を示す。

## 候補と今回の調査深度

表中の「今回深化」は既存ページの短い概要があり、今回資料を追加確認した系統。「新規」は今回の候補として追加した。「評価研究」は製品・保存サービスではなく、他システムを比較する評価枠である。MemoryScope は現行の独立候補ではなく ReMe の旧系譜として扱った。

**長期メモリーと経験記憶の候補**

|                          | 主な記憶単位                                                   | 今回の確認と位置づけ                                           |
| ------------------------ | -------------------------------------------------------- | ---------------------------------------------------- |
| OpenViking               | memory / resource / skill と階層コンテキスト                      | OSS。固定コミットの session、memory lifecycle、retriever を静的確認 |
| MemoryOS                 | 対話ページ、利用者 profile、assistant knowledge                    | OSS と論文。固定コミットの更新・検索・長期保存コードを静的確認                    |
| ReMe（MemoryScope 系譜を含む）  | 人が編集できる Markdown 記憶、session、検索索引                         | OSS。固定コミットの自動記憶ステップと検索コードを静的確認                       |
| MemMachine               | episodic graph、profile、working memory                    | 既存概要を再掲。今回、更新コードや追加資料は未確認                            |
| memU                     | Markdown wiki / skill、agent が準備する記憶                      | 既存概要に現行 README の責任境界を追加確認。サービスが自動抽出するとの誤読に注意         |
| MIRIX                    | core / episode / semantic / procedure / resource / vault | 既存論文概要と固定版 repo README を追加確認。コード監査なし                 |
| Memori（MemoriLabs）       | person / process / session に結び付く fact 等                  | 新規。公式 docs で BYODB と外部 augmentation の分担を確認           |
| Memvid                   | immutable frame と timeline / lexical / vector index      | 新規。公式 docs の v2 portable-file substrate を確認          |
| memsearch（zilliztech）    | Markdown の profile・日誌・skill と由来 transcript               | 新規。固定版 README / getting started。コードの静的監査なし           |
| Acontext                 | task session / artifact と再利用 SOP skill                   | 新規。公式 docs。learning space の削除範囲に注意                   |
| ACE                      | 生成・反省・編集する playbook                                      | 新規。論文・公式 repo 概要。手順文脈の研究                             |
| Memento-Skills           | 読み書きされる structured skill と runtime state                 | 新規。論文・固定版 repo 概要。論文版と後続 runtime を区別                 |
| SkillWeaver              | Web 操作 workflow を skill / API 化                          | 新規。論文・公式 repo。領域限定の手続き記憶                             |
| Agent KB                 | task 制約・行動/推論の trajectory                                | 新規。論文・公式 repo 概要。framework 間の経験検索研究                  |
| MACE                     | agent coordination の機能 subgraph / playbook unit          | 新規。2026-09 論文。極めて新しく未再現                              |
| MemoryData / OpenDataBox | メモリー system / workload / module の評価                      | 新規の評価研究。製品としては数えない                                   |

行ごとの根拠: OpenViking [D-OPENV] [D-LM-OPENV-SESSION] [C-LM-OPENV-SESSION] [C-LM-OPENV-LIFECYCLE] [C-LM-OPENV-RETRIEVER]、MemoryOS [P-MEMORYOS] [D-MEMORYOS] [C-LM-MOS-UPDATER] [C-LM-MOS-RETRIEVER] [C-LM-MOS-LTM]、ReMe [D-LM-REME-FILE] [D-LM-REME-SEARCH] [C-LM-REME-AUTO] [C-LM-REME-SEARCH]、MemMachine [D-MACHINE]、memU [D-MEMU] [D-LM-MEMU-CURRENT]、MIRIX [P-MIRIX] [D-LM-MIRIX-README]、Memori [D-LM-MEMORI]、Memvid [D-LM-MEMVID-INTRO] [D-LM-MEMVID-CLI]、memsearch [D-LM-MEMSEARCH-README] [D-LM-MEMSEARCH-GETTING]、Acontext [D-LM-ACONTEXT-LEARN] [D-LM-ACONTEXT-SPACE]、ACE [P-ACE] [D-LM-ACE-REPO]、Memento-Skills [P-MSKILLS] [D-LM-MSKILLS-REPO]、SkillWeaver [P-LM-SKILLWEAVER] [D-LM-SKILLWEAVER-REPO]、Agent KB [P-LM-AGENTKB] [D-LM-AGENTKB-REPO]、MACE [P-LM-MACE]、MemoryData [P-LM-MEMORYDATA]。

## 固定コミットの静的確認

以下の所見は記載したコミットの選択ファイルに限定する。ライブラリを実行せず、外部ストア、LLM 呼び出し、並行更新、障害時の挙動は観察していない。

**長期メモリーの典型的な更新経路**

1. `capture`

   Session、会話イベント、文書、task artifact を取り込む。保持対象と scope を決める。
2. `transform`

   要約・抽出・手順化を行う。生成物は原資料そのものではなく派生記憶として扱う。
3. `persist/index`

   人が直せる正本、構造化 memory、または file / database と再生成可能な検索索引へ書く。
4. `retrieve`

   scope・時間・候補関連度を使って必要な粒度を読み、応答や次の実行へ渡す。
5. `revise/expire/delete`

   訂正、競合解決、期限処理、source の削除を正本と派生物へ反映し、その状態を評価する。

### OpenViking: session から長期 memory へ

既存 README と VikingMem 論文の説明に加えて、commit `1f4f7039fc394c5d04637828166f4e4e74e249e0` の session、memory lifecycle、階層検索コードと session 文書を読んだ。[D-OPENV] [P-VIKING] [D-LM-OPENV-SESSION] [C-LM-OPENV-SESSION] [C-LM-OPENV-LIFECYCLE] [C-LM-OPENV-RETRIEVER]

Session はタイトル、現在状態、タスクと目標、決定・事実、参照ファイル、失敗と修正、未解決点のような見出し付き working memory を作る。session 終了時に会話を保管し、LLM による要約・記憶抽出を背景処理へ回す経路がある。これは「全文会話をそのまま長期知識にする」方式ではない。要約と抽出で可読性・容量を得る一方、細部・反例が欠落する可能性があり、原会話からの追跡可能性を別途確かめる必要がある。[D-LM-OPENV-SESSION] [C-LM-OPENV-SESSION]

Memory lifecycle は recency の指数減衰と平滑化したアクセス頻度に基づく hotness を計算し、既定半減期を 7 日とする。これは再取得しやすい情報の運用上の優先度であって、真偽・重要性・期限の意味論とは異なる。期限切れを真理値の失効と見なさない。[C-LM-OPENV-LIFECYCLE]

階層 retriever は要約・overview・detail を段階的に使い、memory 以外の resource / skill と同じ仮想パス体系上で文脈を扱う。公式説明のディレクトリ探索と semantic search の組合せは、階層化された長い文書の候補絞り込みに向くが、ここで見た範囲からは型制約・出典付き主張の推論器とは言えない。[C-LM-OPENV-RETRIEVER] [D-OPENV]

### MemoryOS: FIFO の対話ページから profile / knowledge へ

commit `587ed7755c7aed179965792830ff1b5ad9a6fa92` の `memoryos-pypi/` 内から updater、retriever、long-term store、facade を確認した。既存論文は論文の方式と報告結果を示すが、以下のコード所見とは別の根拠である。[P-MEMORYOS] [D-MEMORYOS] [C-LM-MOS-UPDATER] [C-LM-MOS-RETRIEVER] [C-LM-MOS-LTM] [C-LM-MOS-CORE]

短期対話 buffer が上限に達したとき、古い QA を FIFO で取り出し、時刻付き page として保存する。ページには前後リンクを持たせ、話題の連続性を判定して中期の話題セッションへ追加する。話題 summary は LLM に依頼する。この構成は長い対話を区切って要約するが、FIFO の切り出し窓や話題判定が後段の情報粒度を左右する。[C-LM-MOS-UPDATER]

検索時には中期 page、利用者長期情報、assistant knowledge を別々に検索し、上位の話題 page と結合して応答文脈に渡す。Profile は文字列として保存される更新経路があり、長期 knowledge は固定容量 deque と埋め込み類似検索に依存する実装を確認した。これらは paper の抽象的な三層説明より具体的な現行 snapshot の挙動だが、選択ファイルを越える一般化はしない。[C-LM-MOS-RETRIEVER] [C-LM-MOS-LTM]

根拠となる元 QA への参照・長期知識の競合解決・時点指定の検索が、確認した経路でどこまで一貫して維持されるかは不明。固定容量の退避や profile 置換を含め、消失・誤更新を含むデータライフサイクルを導入時に試験する。論文の LoCoMo 数値は著者報告であり、本調査では再現していない。[P-MEMORYOS] [C-LM-MOS-UPDATER] [C-LM-MOS-LTM]

### ReMe: 文書を正本として扱う agent memory

commit `bebad3674573477ad294ca44eb15f508feea2665` の英語設計文書と自動記憶・検索の選択コードを確認した。ReMe は Markdown を編集可能な正本にして、session JSONL・resource・日次メモ・digest と、再構築可能な metadata/index を分ける。README が示す「Memory as File」は、ベクトル DB の内部行だけに情報を閉じ込めない設計である。[D-LM-REME-FILE] [C-LM-REME-AUTO]

自動記憶 step は session を JSONL に保存し、agent wrapper に日次ノートを作成または更新させる。既存ノートの場合、コードが model に渡すのは該当 path であり、追記内容の正しさをコードが決定するわけではない。従って人が読んで修正できる一方、記述の信頼性は agent の生成とレビュー手順に依存する。[C-LM-REME-AUTO]

検索はキーワード検索と任意の vector 検索を並行し、rank fusion で候補を統合する。Markdown ファイルが正本、索引は派生物という分離により、再索引可能性と直接編集を得る。削除や変更を watcher が反映する説明もあるが、今回のコード確認は検索・自動記憶の選択経路に限られ、全 watcher / crash-consistency / 権限処理を監査していない。[D-LM-REME-SEARCH] [C-LM-REME-SEARCH]

README は旧 MemoryScope と過去バージョンを ReMe の系譜に含めている。そのため本調査では MemoryScope を独立した現行候補に重複計上しない。世代差を比較するときは、MemoryScope の旧説明と固定した ReMe version の設計・コードを別々に参照する。[D-LM-REME-FILE]

固定 snapshot から確認した 12 files の対応は次のとおり。hash と commit は各資料の JSON 台帳に記録している。

- OpenViking: `docs/en/concepts/08-session.md` [D-LM-OPENV-SESSION]、`openviking/session/session.py` [C-LM-OPENV-SESSION]、`openviking/retrieve/memory_lifecycle.py` [C-LM-OPENV-LIFECYCLE]、`openviking/retrieve/hierarchical_retriever.py` [C-LM-OPENV-RETRIEVER]
- MemoryOS: `memoryos-pypi/updater.py` [C-LM-MOS-UPDATER]、`memoryos-pypi/retriever.py` [C-LM-MOS-RETRIEVER]、`memoryos-pypi/long_term.py` [C-LM-MOS-LTM]、`memoryos-pypi/memoryos.py` [C-LM-MOS-CORE]
- ReMe: `docs/en/memory_as_file.md` [D-LM-REME-FILE]、`docs/en/memory_search.md` [D-LM-REME-SEARCH]、`reme/steps/evolve/auto_memory.py` [C-LM-REME-AUTO]、`reme/steps/index/search.py` [C-LM-REME-SEARCH]

## 文書・構造データ寄りの追加候補

### MemMachine

既存調査は episodic graph、profile SQL、working memory の分業を README から整理している。[D-MACHINE] 今回は更新コードを固定して監査していないため、グラフの episode と profile の整合性、イベント訂正時に要約・埋め込みへ伝播するかは未確認と明記する。役割ごとに別ストアを採る候補としては有用だが、三種を組み合わせること自体が ontology の統合や出典整合を保証するわけではない。

### memU

既存の pinned README はMarkdown memory / skill の構成を示す。[D-MEMU] 今回確認した現行 README は、host agent が memory や skill を作り、MemoryService は保存・検索を担い独自に LLM/chat を呼ばないと説明する。[D-LM-MEMU-CURRENT] 過去の説明にあるサービス内抽出と混ぜず、取り込み責任・更新内容・embedding をどの主体が提供するかを version 別に確かめる。MemoryScope とは別プロジェクトである。

### MIRIX

論文は Core、Episodic、Semantic、Procedural、Resource、Knowledge Vault の役割を提案し、マルチモーダルな個人アシスタントの memory system を評価する。[P-MIRIX] 現行 repo README はその後の製品・実装説明を持つが、コードは今回未監査で、論文の構成・評価を現行実装が同一に再現するとの意味ではない。[D-LM-MIRIX-README] 複数型を持つ場合でも型間競合と資源由来をどう追跡するかが比較点となる。

### Memori（MemoriLabs）

公式資料は person / process / session scope、facts・preferences・skills・rules・events・execution traces などの抽出対象、semantic recall を説明する。[D-LM-MEMORI] BYODB はデータベースを利用者側に置ける選択肢だが、Advanced Augmentation の処理や LLM / embedding provider の送信先まで自動的にローカルになるとは限らない。データベース配置とモデル処理先を別に確認する。MemoriLabs の製品と `archit15singh/memori` 等の同名 project を混同しない。

### Memvid

公式 docs の v2 は frame を追加単位とする単一 portable file、lexical・vector・temporal index、metadata と履歴を説明し、既存 frame の訂正を旧版置換ではなく新しい frame と状態管理として扱う。[D-LM-MEMVID-INTRO] [D-LM-MEMVID-CLI] この設計は会話の持ち運び・追跡基盤であり、人物同一性・主張の真偽・更新方針をそれだけで決定する memory agent とは異なる。今回コードは取得しておらず、docs に記された機能・制約の確認である。v1 の紹介と現行 Rust/v2 資料は混ぜない。

### memsearch（zilliztech）

公式 README と getting started は Markdown を source of truth、Milvus を再構築可能な検索用 shadow index とする coding-agent 向け設計を示す。[D-LM-MEMSEARCH-README] [D-LM-MEMSEARCH-GETTING] 長期の `MEMORY.md`、日誌、project / user profile、任意で生成する skill を元 transcript の節へ展開して検索する。BM25 と vector の RRF を組み合わせ、local Milvus Lite と外部 Milvus / embedding provider を選ぶ案内がある。`.memsearch/` のログを gitignore にする既定も含め、個人データを共有 repository に載せない運用を確認する。コード監査はしていない。名前が似た MemSearcher や macOS 向け memsearch とは別である。

### Acontext

Acontext は session、artifact を保持し、task 成果から reusable SOP / skill を作る agent memory layer として説明される。[D-LM-ACONTEXT-LEARN] skill は一般的な人物 profile ではなく、タスク実行の手順・注意点の再利用単位である。公式 docs は learning space を削除しても skill/session が削除されないと説明するため、workspace 削除を派生記憶の消去完了と読み替えない。[D-LM-ACONTEXT-SPACE] hosted API / self-host の運用条件、生成 skill の承認・更新・競合処理は導入時に確認する。コードは今回未確認。

## 手順・経験・skill を学習する研究

以下は通常の profile / fact store と異なる。事実を主張として記録するより、タスク条件で実行可能な手順・方策を蓄積する。評価では正答率だけでなく、誤った手順の再利用、実行失敗、環境移行可能性を測る必要がある。

### ACE (Agentic Context Engineering)

ACE は Generator、Reflector、Curator が playbook を提案・診断・統合する方法で、モデル重みの更新ではなくテキスト文脈を反復改善する。[P-ACE] 公式 repository は評価・実装の入口を示す。[D-LM-ACE-REPO] 手順の追記は軽いが、失敗の一般化が誤ると playbook が肥大化またはノイズ化しうる。論文の benchmark 成績は著者報告で、本調査ではコード実行・再現なし。

### Memento-Skills

論文版は構造化 Markdown skills と stateful prompt を使う read/write reflective learning を提案し、モデル重みを更新しない設定の評価を報告する。[P-MSKILLS] 公開 repo は後続の skill-centric runtime へ拡張されている。[D-LM-MSKILLS-REPO] したがって論文の実験 system と 2026-08 の repository runtime は同一版として扱わない。更新・修復を行う手順 memory の比較には有用だが、報告評価は独立に再現していない。

### SkillWeaver

SkillWeaver は Web agent の探索から操作手順を抽出し、再利用できる skill / API として保つ研究である。[P-LM-SKILLWEAVER] 公式 repo は論文方式の公開実装を案内する。[D-LM-SKILLWEAVER-REPO] 対象領域は web workflow であり、個人情報・一般知識の保存システムとして数えない。操作 UI が変化した際の skill の無効化・再検証が導入判断上の論点。

### Agent KB

Agent KB は task の制約、行動・推論列、framework 情報を含む trajectory を検索・再利用する multi-agent knowledge base を提案する。[P-LM-AGENTKB] paper は planner の workflow retrieval、実行 feedback からの診断的 retrieval、embedding disagreement gate、utility に基づく重複処理 / eviction を記載する。80 件の種 trajectory から workflow summary と execution snippet を拡張した報告も paper の実験設定である。公式 repo は公開されているが、今回コードを監査していない。[D-LM-AGENTKB-REPO] framework / tool / domain が異なると transfer が失敗し得る。embedding 類似度の disagreement gate はセキュリティ検証や成功の証明ではない。

### MACE

2026-09 提出の MACE 論文は、agent coordination 用の条件・行動・出力を含む functional subgraph を単位に、support / conflict / repair の関係と実行結果を使い、予算内の playbook を更新する枠組みを提案する。[P-LM-MACE] benchmark 値は非常に新しく、コード・構成・評価を本調査で再現していない。論文付録では比較行の一部に元予測や設定がない既報値を含むとし、robustness protocol も敵対的 security test ではない。一般的な個人記憶製品ではなく、multi-agent 手順記憶研究として扱う。

## 評価研究: MemoryData / OpenDataBox

MemoryData はエージェント memory 製品ではなく、memory system の workload 差とモジュール構成を比較する評価研究である。[P-LM-MEMORYDATA] 論文は storage / representation、extraction、retrieval / routing、maintenance を分解し、12 system と二つの基準方式を五 workload・11 dataset で評価したと報告する。単一 architecture がすべてに優れるとは限らず、ボトルネックが workload ごとに違うという観点を提供する。

この視点は製品を名前だけで比べないために有用である。例えば質問への recall、更新後の正しさ、長期履歴の evidence fidelity、遅延・費用を別指標にする。ただし論文記載の system 列と結果の網羅範囲は同論文の時点・実験条件に限られ、ここでは追試も raw prediction の検証もしていない。製品 inventory では `evaluation-study` として別分類する。

## 導入判断で分ける境界

- **正本と索引:** ReMe / memsearch のように人が読める Markdown を正本、vector / BM25 を再生成可能な索引とできるか。Memvid のように持ち運び可能な frame file を正本にするか。索引が消えても元データを復旧できるか。
- **抽出者:** システム自身が LLM 抽出するのか、host agent が文章を作るのか。memU と ReMe は host 側責任を含む。BYODB でも augmentation が別 provider に送られることはある。
- **経験と事実:** profile / event fact と executable skill / playbook を同じ namespace に混ぜない。前者は出典と時点、後者は対象環境・前提条件・実行結果・再検証期限を持たせる。
- **削除:** source/session、生成 summary、embedding、index frame、cached result、skill/revision を列挙し、削除確認を測る。Acontext の learning space の例のように親単位削除が全派生物を消すとは限らない。
- **権限と送信:** user ID や namespace は識別子であり、認可チェックとは限らない。保存先、embedding provider、LLM augmentation、検索時の prompt inclusion を別々に確認する。
- **Ontology:** 型名があるだけで標準 ontology とはならない。複数製品をまたいで Person、Project、SourceRevision、Claim、Event、Skill の意味と時刻を共有する必要が出てから、明示した ontology へ mapping する。

### 名称・系譜の取り違えを避ける

- **MemoryScope** は今回、現行 ReMe と並列の製品ではなく旧系譜として整理した。[D-LM-REME-FILE]
- **Memori** は `MemoriLabs/Memori` の system を指す。似た名前の別 GitHub repository は別候補である。[D-LM-MEMORI]
- **memsearch** は `zilliztech/memsearch`。MemSearcher の研究 agent、macOS 向けアプリ等を指さない。[D-LM-MEMSEARCH-README]
- **Memento-Skills** は structured skill/runtime 系。推論時の要約・KV state を扱う同名 MEMENTO 研究とは別の対象である。[P-MSKILLS]
- **VikingMem** は OpenViking の論文系譜、OpenViking は公開リポジトリの現行実装。論文図の各機能が実装済みとは推定しない。[P-VIKING] [D-OPENV]

## 確認範囲と限界

公式文書・論文・README は 2026-09-29 時点に参照した。固定コミットの byte/hash を記録した static review は OpenViking 4 files、MemoryOS 4 files、ReMe 4 files の計 12 files。`snapshots.json` は固定 SHA と取得ファイルを記録する。全ファイル精読、コード実行、インストール、サービス API 呼び出し、外部 LLM 送信、benchmarks は行っていない。

静的確認により、選択ファイルの中の処理経路とデータ表現は特定できるが、本番環境の安全性、性能、同時実行、永続ストア間の整合性を保証しない。公式 docs の機能説明は provider の説明であり、独立検証とは異なる。論文の数値は著者報告として記録し、今回の測定結果と混同しない。評価計画は[共通評価](../evaluation.md)、既存候補の境界は[追加候補](additional-systems.md)、全体の比較は[拡張ランドスケープ](extended-landscape.md)へ。

## 読み終わった後に確認する問い

1. 選んだ方式が保存するのは会話、出典付き事実、実行経験、skill のどれか。用途が複数なら責任境界はどこか。
2. 間違いを訂正または削除したとき、正本・抽出済み memory・検索 index・skill をどう更新し、どの単位で復旧できるか。
3. 論文、README、現行 code、実行済み評価のうち、各主張を直接支えている根拠はどれか。

[D-OPENV]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/README.md

[P-VIKING]: https://arxiv.org/abs/2605.29640v3

[P-MEMORYOS]: https://arxiv.org/abs/2506.06326v1

[D-MEMORYOS]: https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/README.md

[D-MACHINE]: https://github.com/MemMachine/MemMachine/blob/d57f5cb36a357c01085f571a0cdd2dfbc9882f89/README.md

[D-MEMU]: https://github.com/NevaMind-AI/memU/blob/2c050bc9681a4c0aff1af211a000e73d14f33356/README.md

[P-MIRIX]: https://arxiv.org/abs/2507.07957v1

[D-LM-OPENV-SESSION]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/docs/en/concepts/08-session.md

[C-LM-OPENV-SESSION]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/openviking/session/session.py

[C-LM-OPENV-LIFECYCLE]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/openviking/retrieve/memory_lifecycle.py

[C-LM-OPENV-RETRIEVER]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/openviking/retrieve/hierarchical_retriever.py

[C-LM-MOS-UPDATER]: https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/updater.py

[C-LM-MOS-RETRIEVER]: https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/retriever.py

[C-LM-MOS-LTM]: https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/long_term.py

[C-LM-MOS-CORE]: https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/memoryos.py

[D-LM-REME-FILE]: https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/docs/en/memory_as_file.md

[D-LM-REME-SEARCH]: https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/docs/en/memory_search.md

[C-LM-REME-AUTO]: https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/reme/steps/evolve/auto_memory.py

[C-LM-REME-SEARCH]: https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/reme/steps/index/search.py

[D-LM-MEMU-CURRENT]: https://github.com/NevaMind-AI/memU/blob/main/README.md

[D-LM-MIRIX-README]: https://github.com/Mirix-AI/MIRIX/blob/8cb06a62bbb7c478beb33dd4f2815696a72df482/README.md

[D-LM-MEMORI]: https://memorilabs.ai/docs/memori-byodb/concepts/how-memory-works/

[D-LM-MEMVID-INTRO]: https://docs.memvid.com/introduction/glossary

[D-LM-MEMVID-CLI]: https://docs.memvid.com/cli

[D-LM-MEMSEARCH-README]: https://github.com/zilliztech/memsearch/blob/2a4652fa086fbd45e92bfd8da7781ebe1642baa7/README.md

[D-LM-MEMSEARCH-GETTING]: https://github.com/zilliztech/memsearch/blob/2a4652fa086fbd45e92bfd8da7781ebe1642baa7/docs/getting-started.md

[D-LM-ACONTEXT-LEARN]: https://docs.acontext.io/learn/quick

[D-LM-ACONTEXT-SPACE]: https://docs.acontext.io/learn/learning-spaces

[P-ACE]: https://arxiv.org/abs/2510.04618v3

[D-LM-ACE-REPO]: https://github.com/ace-agent/ace/tree/82709de050e1db6e6ef2f07bcb0393560b94992a

[P-MSKILLS]: https://arxiv.org/abs/2603.18743v1

[D-LM-MSKILLS-REPO]: https://github.com/Memento-Teams/Memento-Skills/tree/ee9b9a45efd093d669c06fe318b4b1dceb246d19

[P-LM-SKILLWEAVER]: https://arxiv.org/abs/2504.07079v1

[D-LM-SKILLWEAVER-REPO]: https://github.com/OSU-NLP-Group/SkillWeaver

[P-LM-AGENTKB]: https://arxiv.org/abs/2507.06229v5

[D-LM-AGENTKB-REPO]: https://github.com/OPPO-PersonalAI/Agent-KB

[P-LM-MACE]: https://arxiv.org/abs/2609.21533v1

[P-LM-MEMORYDATA]: https://arxiv.org/abs/2606.24775v1
