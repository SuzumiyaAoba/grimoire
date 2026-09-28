# PageIndex 調査 — 文書構造を使う検索とメモリー基盤での役割

[調査トップ](../README.md) / [システム比較](README.md) / [資料台帳](../sources.md)

**確認日: 2026-09-29。対象: [VectifyAI/PageIndex](https://github.com/VectifyAI/PageIndex) の公開コードと公式資料。** コードは `619cbd89f6dd02681a8cfc5d00b1f9b4848e973e`（`2026-09-28 14:01:02 UTC`）に固定した。manifest の版は `0.2.10`。この版が PyPI で配布済みかは別途確認していない。インストール、LLM API 呼び出し、ベンチマーク再実行は行っていない。[C-PI-MANIFEST]

2026-08-26 の公式発表では、Flash による高速な索引生成と SDK のローカル利用が案内されている。本稿はこの追加後の実装を中心に扱い、従来の索引生成経路との違いも示す。[D-PI-FLASH]

PageIndex は、長い文書を章・節・ページの階層として索引化し、LLM が構造と本文を読みながら回答に必要な根拠を探す仕組みである。文書を継続的に参照する外部メモリーの**検索層**として検討できる。業務概念の意味を定義するオントロジーや、事実の訂正・失効を管理する機構は、別の層で設計する必要がある。[D-PI-START] [C-PI-TOOLS]

**調査で押さえる三点**

1. `CONFIRMED` **現行 OSS は索引生成から検索・回答までを含む** — `PageIndexClient`、ローカル索引・保存、文書を読むツール、回答エージェントが公開されている。旧来の `run_pageindex.py` だけを見て「OSS は索引作成のみ」と説明すると、今回確認した版を取り違える。[C-PI-CLIENT] [C-PI-LOCAL] [C-PI-CHAT]
2. `CONFIRMED` **Flash の構造抽出と LLM による加工は別工程** — PDF のレイアウトや埋め込み目次から初期構造を得る処理は LLM を使わない。通常の SDK 経路では、その後にノードの要約、木の整理・拡張、文書説明の生成を行うため、索引作成にも LLM が必要になる。[C-PI-FLASH-API] [C-PI-OPT] [C-PI-LOCAL]
3. `UNVERIFIED` **性能値は条件付きの開発元報告** — FinanceBench の 98.7% と、現行ローカル SDK の OSS ベンチマークは別の評価である。報告値を自社文書での正答率や、すべてのベクトル検索方式に対する優位とみなさない。詳しい条件は後述する。[D-PI-README]

## 1. 基本概念と検索の考え方

通常のベクトル検索では、文章を embedding という数値表現に変換し、質問との近さで候補を探す。PageIndex の文書内検索では、目次に相当する tree index を読み、どの節・ページに答えがありそうかを LLM に判断させる。RAG は取得した資料を回答生成に使う構成の総称なので、embedding を使わない PageIndex も RAG に含まれる。[D-PI-START] [C-PI-TOOLS]

たとえば「営業利益率が前年より下がった理由」を探す場合、以下の構造から業績説明と財務諸表のページを読み合わせる。これは説明用の例であり、実際に生成した索引や回答ではない。

```
年次報告書
├── 事業概要
├── 経営成績
│   ├── 売上・利益の変動
│   └── セグメント別の状況
└── 財務諸表
    ├── 損益計算書
    └── 注記
```

**vectorless** は、この検索経路に embedding 索引やベクトル DB を必須としないという意味で読む。**no chunking** も、本文を一切区切らないという意味ではない。実装にはページ範囲、節、ノード、長い節の細分化、ツール応答の分割がある。任意の固定長 chunk を検索の基本単位にする方式との違いである。[C-PI-FLASH-API] [C-PI-OPT] [C-PI-TOOLS]

木は一般に章・節の包含関係を表す。ここに枝があることだけで、人物の所属、製品の互換性、クラスの公理を表す知識グラフや OWL オントロジーになるわけではない。用語の違いは[オントロジーの基礎](../foundations/ontology.md)と[メモリーの基礎](../foundations/memory.md)を参照。

## 2. 現行ローカル版の処理

**入力 PDF から根拠付き回答まで**

1. `1. PDF と構造を読む`

   `LocalAPI.submit_document()` は PDF を検査し、ページ本文を抽出する。既定の Flash はレイアウト由来の見出しと、利用可能な埋め込み目次から木を作る。読み取れる本文が必要で、ローカル経路で OCR は実行しない。[C-PI-LOCAL] [C-PI-FLASH-API]
2. `2. 木を整理し、要約する`

   既定の最適化は短い構造の merge と、長い節に対する LLM の expand を組み合わせる。merge は決定的な処理で、expand は本文上にある見出しかを確認する。ノード要約と文書説明も生成する。[C-PI-OPT] [C-PI-LOCAL]
3. `3. ローカルストアに保存する`

   文書 ID ごとに `doc.json`、`tree.json`、`pages.json` を保存し、`manifest.json` を文書一覧のキャッシュとして使う。既定の保存先は `./.pageindex`。この保存処理は、入力 PDF の原本アーカイブとは別である。[C-PI-STORE] [D-PI-CLIENT]
4. `4. 質問に応じて構造とページを読む`

   LLM が `browse_documents`、`get_document`、`get_document_structure`、`get_page_content` を利用する。確認したローカルの既定指示では、20 ページ超はまず構造を読み、20 ページ以下は本文を直接読める。回答と引用は読んだページを根拠に生成する。[C-PI-TOOLS] [C-PI-CHAT]

「tree search」を、常に根から一つずつ枝を降りる固定アルゴリズムや、計算量が必ず `O(log N)` になる手法と解釈しない。公開経路では構造・ページを読むツールを LLM が選ぶ。探索回数は質問、木の形、モデル、必要な根拠の数に依存する。`tree_optimize.py` のページ単位の探索コストは構造を整理するための代理尺度であり、実測の API 料金や遅延そのものではない。[C-PI-TOOLS] [C-PI-OPT]

### 保存される木と公開 API の表現

Flash の木は、主に `title`、`node_id`、`start_index`、`end_index`、`summary`、子を表す `nodes` で構成される。ページ範囲は PDF の物理ページを 1 始まりで表す。紙面に印刷されたページ番号と一致するとは限らない。[D-PI-FLASH-README]

`get_tree()` の公開形式は `start_index` を `page_index` に変換し、内部の `end_index` をそのまま返す形式ではない。`node_summary=True` では葉に `summary`、子を持つノードに `prefix_summary` が入る。小さな索引だけを取得するには `node_summary=True, include_text=False` を明示する。既定の `include_text=True` では本文も返る。[C-PI-LOCAL] [D-PI-DOCS]

### 失敗時と従来方式

Flash で階層を検出できない場合、ページごとのノードへフォールバックする。ただし、その平坦な結果が **10 ノードを超えると**ローカル SDK と CLI は受け付けず、`standard` の利用を案内する。Flash が失敗したら必ず自動的に別方式へ切り替わるわけではない。[C-PI-FLASH-API] [C-PI-CLI]

`standard` は従来の、LLM を用いる目次・開始ページの推定と検証を含む経路として残っている。また CLI に Markdown 用の `--md_path` があるが、これは `PageIndexClient.submit_document()` が Markdown を受け付けることを意味しない。現行ローカル SDK の文書投入は PDF に限定される。[C-PI-CLASSIC] [C-PI-CLI] [C-PI-LOCAL]

## 3. OSS・Cloud・エージェント統合の境界

**確認した提供範囲**

|         | ローカル OSS                                | PageIndex Cloud              | 判断時の補足                                        |
| ------- | --------------------------------------- | ---------------------------- | --------------------------------------------- |
| 文書入力    | SDK はテキストを持つ PDF                        | 公式 API は PDF・PPTX・Word       | CLI の Markdown 索引生成は別入口。                      |
| 索引と保存   | 自分の環境で処理しディスク保存                         | 提供者が解析・索引・保存を管理              | いずれも索引と回答モデルを分けて設定できる。                        |
| 画像・スキャン | OCR なし、テキスト抽出が前提                        | OCR・画像理解を提供すると説明             | 表・図の正確さは実データで測る。                              |
| 引用      | ページ単位                                   | 対応文書ではブロック単位も利用              | 引用先があることと、その主張が正しいことは別。                       |
| 文書の組織化  | 複数文書の参照が可能、folder は未対応                  | metadata schema、folder、文書間探索 | ローカルで metadata を保存できる実装上の補足は下記。               |
| MCP     | Claude Agent SDK 向け in-process MCP 統合あり | managed remote MCP を提供       | README の Cloud 限定表記を、あらゆるローカル MCP 統合の不在と読まない。 |

入力・保存・OCR の条件は公式の導入・文書処理・client 設定と公開コード、引用は LLM Integration、MCP の区別は Agent Integration に基づく。[D-PI-START] [D-PI-DOCS] [D-PI-CLIENT] [C-PI-CLI] [C-PI-LOCAL] [D-PI-CHAT] [D-PI-AGENTS]

**metadata の資料差。** 公式 docs の Metadata 節は Cloud として説明している。一方、固定した `LocalAPI.submit_document()` は JSON 化できる `metadata` を受け取り保存する。ローカルでの保存はコード確認できるが、Cloud の schema 検証や文書検索機能と同じ保証はない。資料の更新時点と実装の差として扱う。[D-PI-DOCS] [C-PI-LOCAL]

**文書指定と権限。** 外部エージェントに渡す `document_context()` / `folder_context()` は、どこを探すかを伝える文字列であり、ツールのアクセス制限ではない。固定コードでは内蔵ローカル `chat(doc_id=...)` に許可 ID 集合を渡す経路がある一方、Cloud の自前モデルによる回答経路ではその制限を外し、プロンプトで対象を示す。したがって、文書指定パラメーターを一律に認可境界とみなせない。複数利用者の資料を扱う場合は、認可済みのストア・資格情報・ツール境界をアプリケーション側で設計する。[D-PI-AGENTS] [C-PI-CLIENT] [C-PI-TOOLS]

**文書間の大規模検索。** PageIndex File System は文書の上に仮想ノードや質問に応じた階層を置く方式として紹介されている。百万文書規模は Enterprise / Cloud に関する提供者の主張で、ローカル OSS の規模試験を確認した結果ではない。ローカルの複数文書指定を、その機能全体と同一視しない。[D-PI-FILESYSTEM] [D-PI-README]

## 4. 性能主張と評価の読み方

**異なる評価を分けて読む**

|              | 開発元の報告                        | 確認できる条件と限界                                                                                     |
| ------------ | ----------------------------- | ---------------------------------------------------------------------------------------------- |
| FinanceBench | 98.7%                         | 詳細資料の対象は PageIndex を使う Mafin 2.5 全体。公開セットを共通の文書集合から検索し、曖昧な問題には専門家の判定を用いると説明。現行 OSS 単体の再現値ではない。 |
| OSS の検索・読解   | 62 問、34 PDF、1,945 ページ         | MMLongBench-Doc-V2 由来の本文中の事実検索。図・表・計数・算術を除き、Flash で索引化できない文書も除外。抽出失敗まで含む全体精度ではない。              |
| ローカルの索引費用・時間 | 約 0.001 ドル / ページ、約 13 秒〜4.5 分 | README の指定モデルと 9〜1,098 ページの 9 文書に関する報告。モデル料金・並列数・本文密度を変えた費用保証ではない。                             |

上表の出典は、FinanceBench 評価、OSS Benchmark、本体および Flash の各 README である。[D-PI-FINANCE] [D-PI-OSS-BENCH] [D-PI-README] [D-PI-FLASH-README]

OSS ベンチマークでは、索引用モデルと木を共通にして回答モデル・reasoning effort を変えている。例として `gpt-5.6-luna / high` は 60/62 問、平均 0.0036 ドル / 問、`gpt-5.6-sol / high` は 62/62 問、0.0819 ドル / 問と報告する。料金は回答処理分で、共通の索引作成費用を含まない。採点は参照回答と生成回答の意味的一致を判定する方式である。この表だけではベクトル検索との同条件比較にはならない。[D-PI-OSS-BENCH]

FinanceBench 用の公開 `eval.py` は既定で GPT-4o を判定器に使い、丸めや妥当な推論・解釈を許容する指示を含む。結果ファイルを採点するコードを公開していることと、Mafin 2.5 の検索から回答までを同じ条件で再構築できることは区別する。README の比較表には評価対象の範囲や結果公開の有無が異なる方式が混在する。したがって、98.7% を他方式に対する一般的な性能保証として用いない。[D-PI-FINANCE-EVAL] [D-PI-FINANCE]

本調査では、いずれの結果も再実行していない。公式の高い正答率を採用理由の一つにする場合も、索引化を拒否された文書を含む自社データでの評価を追加する。

## 5. 最小の利用例と依存関係

以下は固定コードと API 文書を照合した**未実行の例**である。Python 3.10 以上が manifest の要件。再現調査では浮動の最新版指定より、確認したコミットと依存の解決結果を記録するとよい。次のインストール例はソース版を固定するが、推移的な依存すべてを固定するものではない。[C-PI-MANIFEST]

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install "git+https://github.com/VectifyAI/PageIndex.git@619cbd89f6dd02681a8cfc5d00b1f9b4848e973e"
```

モデル名は、利用できるモデルを環境変数で指定する。モデル提供者の API キー・接続先も別途設定する。`INDEX_MODEL` / `CHAT_MODEL` はこの例用の変数で、PageIndex が自動的に読む予約名ではない。[D-PI-CLIENT]

```python
import os
from pageindex import PageIndexClient


def main():
    client = PageIndexClient(
        index={
            "model": os.environ["INDEX_MODEL"],
            "storage_path": "./.pageindex",
        },
        chat=os.environ["CHAT_MODEL"],
    )
    doc_id = client.submit_document("./report.pdf")["doc_id"]
    # ローカルでは索引の完成後に戻る。以後は doc_id を保持して再利用する。
    tree = client.get_tree(doc_id, node_summary=True, include_text=False)
    print(tree)
    answer = client.chat(
        "営業利益が前年から変化した理由を、根拠ページとともに説明してください。",
        doc_id=doc_id,
        citations=True,
    )
    print(answer)


if __name__ == "__main__":
    main()
```

Flash の並列処理を起動するスクリプトでは、上のように `__main__` ガードを付ける。通常の索引生成・回答には LLM 呼び出しがあり、費用とデータ送信が発生し得る。[C-PI-LOCAL] [C-PI-CHAT]

構造抽出のみを確認したい場合、低水準 API の `page_index_flash("report.pdf", summary=False, optimize=False)` は要約と拡張を止める LLM 不使用の経路である。これは通常の SDK による索引保存・検索・回答の完了を意味しない。[C-PI-FLASH-API]

**manifest に宣言された主な依存**

|                                          | 役割                            | 確認した宣言                                                  |
| ---------------------------------------- | ----------------------------- | ------------------------------------------------------- |
| Python                                   | 実行環境                          | >=3.10                                                  |
| pypdfium2 / PyPDF2                       | Flash の PDF 処理 / ローカルのページ本文抽出 | pypdfium2 >=5、PyPDF2 >=3.0.0                            |
| LiteLLM / OpenAI SDK / OpenAI Agents SDK | LLM 接続と回答エージェント               | litellm >=1.97.0、openai >=1.70.0、openai-agents >=0.18.1 |
| requests / urllib3 / mcp                 | HTTP と MCP 接続                 | requests >=2.28.0、urllib3 >=1.26、mcp >=1.19.0,<3        |
| Anthropic / Claude Agent SDK             | 任意の追加統合                       | extras の anthropic / claude                             |

表はインストール済みの版ではなく `pyproject.toml` の宣言である。`requirements.txt` には一部固定値もあり、両者は同じ lockfile ではない。リポジトリ本体のライセンスは MIT。依存ライブラリや利用するモデル・サービスの条件まで一律に MIT になるわけではない。[C-PI-MANIFEST] [C-PI-REQUIREMENTS] [C-PI-LICENSE]

## 6. 制約、更新、削除

**検索品質。** 見出し抽出、ページ本文、要約、探索判断、最終回答の各段階で誤りが起こり得る。要約で目立たない注記、図にしかない数値、複数箇所に分散した根拠などを確認対象にする。これは構造から導く評価上の注意であり、本調査で測定した失敗率ではない。ベクトル検索でも引用・会話を使った質問書き換え・reranker・隣接箇所の取得を組み合わせられるため、公式の対比表をすべての RAG 実装に一般化しない。

**費用と遅延。** embedding 作成・ベクトル DB を省けても、木の生成・要約と検索時の LLM 呼び出しは残る。質問ごとの費用はモデル、探索回数、読み込む本文、キャッシュに左右される。費用比較では `索引作成 + 質問回数 × 検索・回答 + 再索引 + 保管・運用` を同じ期間で評価する。[C-PI-LOCAL] [C-PI-CHAT]

**データの送信先。** ローカル版は索引と抽出テキストを自分のディスクに置くが、外部 LLM を設定すれば、要約・構造拡張・回答に使う文書内容がその提供者へ送られる。ローカル保存を、文書由来データが常に端末外へ出ない保証と読み替えない。閉じた環境が必要ならモデル接続先も含めて確認する。[C-PI-UTILS] [C-PI-CHAT] [D-PI-CLIENT]

**更新。** 確認したローカル投入経路は、新しい UUID を発行して文書全体を索引化する。同名ファイルには別名を付ける。既存文書の差分更新、旧版を自動失効させる規則、有効時点に応じた版の選択は、この経路では確認していない。アプリケーション側で文書ハッシュ、版、取り込み時点、原本との対応、検索対象の切り替えを管理する設計が必要になる。[C-PI-LOCAL]

**削除。** ローカルストアは `doc.json` を外して読取対象から除き、文書ディレクトリと manifest の項目を削除する。ディレクトリ削除には `ignore_errors=True` があるため、成功応答を媒体上の完全消去と同一視しない。呼出側の原本、外部 LLM のログ、独自キャッシュやバックアップまでの削除は別に扱う。Cloud の削除 API は資料で確認したが、その内部の完全性は未検証である。[C-PI-STORE] [D-PI-DOCS]

## 7. オントロジー・メモリー・関連方式との関係

以下は仕組みに基づく位置づけで、同条件の優劣を測った結果ではない。関連方式の根拠と確認版は[知識検索の比較](knowledge-retrieval.md)と[構造化メモリーの比較](structured-memory.md)に記載している。

**何を構造化する方式か**

|             | 中心となる構造                    | PageIndex と組み合わせる意味                |
| ----------- | -------------------------- | ---------------------------------- |
| PageIndex   | 文書の章・節・ページと要約              | 原資料のどこを読むかを決める。                    |
| RAPTOR      | chunk のクラスタリングと再帰的要約の木     | 同じ木でも、PDF の見出しを出発点とする方式とは構成原理が異なる。 |
| GraphRAG    | 抽出した実体・関係・community report | 文書集合全体のテーマや実体関係を扱う検索と比較できる。        |
| OWL / SHACL | 概念・関係の意味、公理 / データの制約       | 読み出した本文から作る型付き主張の意味と検証を担当する。       |
| 時間付きメモリー    | 出来事・主張・有効期間・根拠             | 新しい資料で旧情報をどう改めるか、過去時点に何を答えるかを担当する。 |

本リポジトリの知識基盤へ適用するなら、次の分担が考えられる。これは**本調査の設計案**であり、PageIndex の組み込み機能の一覧ではない。

**文書検索から知識更新へ接続する案**

1. `原資料と索引を登録する`

   PDF 原本・ハッシュ・版・公開日・取得日時・権限を別に保持し、PageIndex の文書 ID と対応付ける。
2. `根拠ページを読む`

   PageIndex で候補節を見つけ、本文を読む。回答や抽出主張には原本の版とページを残す。
3. `型付き主張として検証する`

   人物、製品、組織などの実体を同定し、主張・出典・有効期間を表現する。必要なら SHACL で形式的な制約を検査する。
4. `訂正と再索引を連動させる`

   資料が更新されたら新しい索引と旧版の対応を管理し、旧資料に依存する主張・要約・回答キャッシュの扱いを決める。

会話履歴を `chat()` へ渡せることは、利用者ごとの事実を自動抽出し、矛盾を解決し、継続的に忘却するメモリー管理を備えることとは別である。この区別は[知識更新](../foundations/knowledge-updates.md)と[応用例](../foundations/ontology-memory-applications.md)に接続する。[D-PI-CHAT]

## 8. 採用を判断する小規模評価

まず、章立てのある長いテキスト PDF を繰り返し参照する用途で比較する。スキャン・図表中心、短い記録の大量検索、頻繁な部分更新が中心の場合は、その前提で適性を検証する。以下は未実施の評価案である。

**同じ文書・回答モデルで比較する観点**

|       | 測るもの                            | 失敗を分ける方法                                  |
| ----- | ------------------------------- | ----------------------------------------- |
| 抽出と索引 | 本文欠落、見出し・ページ範囲、拒否された文書の割合       | PDF 表示と抽出本文・木を人手照合する。                     |
| 検索    | 正解の根拠ページを取得できた割合、不要ページ数         | 単一節、別表・注記、複数節、日本語、同名の複数年度を分ける。            |
| 回答と引用 | 正答、引用先の存在、引用内容が回答を支えるか          | 根拠不足の質問に適切に回答を控えるかも測る。                    |
| 費用と速度 | 索引費用、質問単価、LLM 呼出回数、p50 / p95 遅延 | モデル、reasoning effort、キャッシュ、同時実行数を固定・記録する。 |
| 更新と権限 | 新版への切替、旧版への誤参照、削除後の読取、許可外文書の取得  | 内蔵 local chat、Cloud、自作エージェントを別々に検証する。     |

比較対象には、BM25、ベクトル検索、両者の併用と reranker、文書全体をモデルに渡す方式を置く。回答モデル、入力 PDF の解析品質、根拠集合、質問、採点基準を揃える。PageIndex だけに強いモデルや良い OCR を使った比較では、索引・検索方式の効果を分離できない。詳細な評価設計は[共通の評価方法](../evaluation.md)を参照。

## 関連ドキュメントと確認範囲

- [文書のグラフ検索と企業向け知識基盤](knowledge-retrieval.md)
- [メモリーの基礎と応用](../foundations/memory.md)
- [オントロジーとメモリーを組み合わせる実例](../foundations/ontology-memory-applications.md)
- [資料台帳](../sources.md) / [取得コミットとファイルハッシュ](../evidence/repository-snapshots.json)

確認したのは選択した公開ファイルの静的な処理と公式文書の関連箇所である。Cloud の非公開アルゴリズム、すべてのモデル・SDK 統合、全依存の動作、性能値、日本語 PDF の品質、削除・認可の運用上の完全性は検証していない。取得ファイル数と精読範囲は同一ではなく、出典ごとの `review_depth` を資料台帳に記録する。

[C-PI-MANIFEST]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pyproject.toml

[D-PI-FLASH]: https://pageindex.ai/blog/pageindex-flash

[D-PI-START]: https://docs.pageindex.ai/getting-started

[C-PI-TOOLS]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/agent_tools.py

[C-PI-CLIENT]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/client.py

[C-PI-LOCAL]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_api.py

[C-PI-CHAT]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_chat.py

[C-PI-FLASH-API]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/flash/api.py

[C-PI-OPT]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/tree_optimize.py

[D-PI-README]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/README.md

[C-PI-STORE]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_store.py

[D-PI-CLIENT]: https://docs.pageindex.ai/sdk/client

[D-PI-FLASH-README]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/flash/README.md

[D-PI-DOCS]: https://docs.pageindex.ai/sdk/documents

[C-PI-CLI]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/run_pageindex.py

[C-PI-CLASSIC]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/page_index_classic.py

[D-PI-CHAT]: https://docs.pageindex.ai/sdk/chat

[D-PI-AGENTS]: https://docs.pageindex.ai/sdk/agents

[D-PI-FILESYSTEM]: https://pageindex.ai/blog/pageindex-filesystem

[D-PI-FINANCE]: https://github.com/VectifyAI/Mafin2.5-FinanceBench/blob/1c890d5e0fd9929953d38282614555847727011d/README.md

[D-PI-OSS-BENCH]: https://github.com/VectifyAI/PageIndex-OSS-Benchmark/blob/ad4c0b92970a6f4801f09ff2e647389e8f5874fa/README.md

[D-PI-FINANCE-EVAL]: https://github.com/VectifyAI/Mafin2.5-FinanceBench/blob/1c890d5e0fd9929953d38282614555847727011d/eval.py

[C-PI-REQUIREMENTS]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/requirements.txt

[C-PI-LICENSE]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/LICENSE

[C-PI-UTILS]: https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/utils.py
