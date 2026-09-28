# オントロジーの基礎と設計

[調査トップ](../README.md) / [基本概念](concepts.md) / [資料台帳](../sources.md)

**確認日: 2026-09-29。** ここでは、共有された語彙と形式的な意味づけを扱う工学的なオントロジーを、基礎用語から実装上の選択まで説明する。後半の例に出る人物・組織・発言は説明用の架空データである。

## オントロジーとは何か

哲学でいう存在論（ontology）は「何が存在するか」「存在者をどう分類するか」を問う分野である。情報科学・知識表現でいうオントロジーは、特定の領域について、共有する概念・関係・制約・公理を明示したモデルを指す。Gruber の定式化は、共有された概念化を形式的に記述する仕様として、クラスや関係などの語彙を挙げている。[D-GRUBER]

実務では、オントロジーは「名前一覧」より多くを含む。`Person` が何を表すか、`worksFor` の両端にどんな種類のものが来るか、`Employee` は `Person` の一種か、といった意味を機械で処理できる形で決める。Noy と McGuinness は、すべての目的に正しい唯一のオントロジーがあるわけではなく、用途や必要な問いに合わせて設計し、評価する考え方を説明している。[D-ONTO101]

### 近い言葉との違い

**オントロジーと関連概念**

|         | 主な役割                | オントロジーとの関係                        |
| ------- | ------------------- | --------------------------------- |
| オントロジー  | 領域の語彙とその意味、公理を共有する  | 他の形式で表現することもできる                   |
| タクソノミー  | 概念を上位・下位に分類する       | オントロジーの一部になりうるが、関係や制約をすべて表すとは限らない |
| データスキーマ | 保存データの構造や型、必須項目を定める | オントロジーと重なるが、形式適合性の検査を主目的にすることがある  |
| ナレッジグラフ | 実体と実体間の事実をグラフで表す    | オントロジーは、そのグラフの語彙や推論規則を与えうる        |

これらは排他的な製品分類ではない。同じシステムに、RDF のデータ、OWL の公理、SHACL の検証形状、検索用のスキーマを置ける。役割を分けて説明することで、「意味が導けること」と「入力データが要件に合うこと」を混同しにくくなる。

## 基本要素

| 要素             | 意味             | 例                                  |
| -------------- | -------------- | ---------------------------------- |
| クラス（class）     | 個体の種類を表す概念     | `Person`, `Organization`           |
| 個体（individual） | 領域にある特定の対象     | `ex:aoi`, `ex:aozora-lab`          |
| オブジェクトプロパティ    | 個体から個体への関係     | `ex:worksFor`                      |
| データプロパティ       | 個体から値への関係      | `ex:label`, `ex:startDate`         |
| リテラル           | 文字列、数、日付などの値   | `"葵"@ja`, `"2026-04-01"^^xsd:date` |
| 公理（axiom）      | 用語や個体の意味を定める記述 | `Employee` は `Person` の下位クラス       |

識別子は表示名と分ける。`ex:aoi` は対象の識別に使い、`"葵"@ja` は日本語ラベルとして使う。ラベルの変更があっても識別子の同一性まで変わるとは限らない。

### TBox・RBox・ABox

TBox はクラスなどの語彙と分類公理、RBox はプロパティの性質や階層、ABox は個体についての主張を説明するための区分である。たとえば「社員は人である」は TBox、「直属の勤務先関係は所属先関係の一種である」は RBox のプロパティ階層、「葵は人である」は ABox の個体に関する主張にあたる。実装上、これらを別ファイルや別データベースに分割する必要はない。OWL では同じオントロジーに語彙・公理・個体の記述を含められる。[S-OWL]

## RDF と小さな例

RDF はデータを主語・述語・目的語の triple として表現する。RDF 1.1 では、主語は IRI または空白ノード、述語は IRI、目的語は IRI・空白ノード・literal である。RDF graph は triple の集合であり、複数の graph を dataset としてまとめることもできる。[S-RDF]

以下は Turtle 形式の例である。`ex:` は説明用の独自語彙を示す。

```turtle
@prefix ex: <https://example.org/memory/> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:Person a owl:Class .
ex:Organization a owl:Class .
ex:Employee rdfs:subClassOf ex:Person .
ex:worksFor a owl:ObjectProperty ;
    rdfs:domain ex:Person ;
    rdfs:range ex:Organization .

ex:aoi a ex:Person ;
    ex:worksFor ex:aozora-lab .
ex:aozora-lab a ex:Organization ;
    rdfs:label "青空研究所"@ja .
```

この graph では、クラスとその下位クラス、関係、二つの個体、ラベルを表現している。RDFS の `domain` と `range` は、関係を持つ主語・目的語がそれぞれ指定クラスの個体であるという含意を与える。値が範囲外なら入力エラーにする、という検証制約ではない。たとえば別のクラスとして型付けされた対象が `worksFor` の目的語になっていても、RDFS は通常それを拒否せず、指定された型も含意する。[S-RDFS]

この二項関係は、用語と triple の形を学ぶための最小モデルである。時点や根拠も記録する応用では、[応用編](ontology-memory-applications.md)のように主張ノードへ置き換える設計にできる。

## 標準の役割を分ける

| 標準                | 主な役割                                 | 注意点                                               |
| ----------------- | ------------------------------------ | ------------------------------------------------- |
| RDF               | 識別子、literal、triple、graph/dataset     | graph 名だけでは出典や時刻の意味は決まらない。[S-RDF]                 |
| RDFS              | クラス階層、プロパティ階層、domain/range による基本的な含意 | domain/range はデータ検証の必須値制約ではない。[S-RDFS]            |
| OWL 2             | クラス・関係・個体の論理的公理と含意                   | 欠落項目の検査を行う一般的なスキーマ言語ではない。[S-OWL]                  |
| SHACL             | データ graph が指定 shape に適合するかを検証        | 整形式であることは、主張が真であることを保証しない。[S-SHACL]               |
| SPARQL            | RDF graph へのパターン検索・集約                | 推論済みデータも検索するかは endpoint の設定に依存する。[S-SPARQL-QUERY] |
| SKOS              | 分類体系・シソーラスなどの概念、ラベル、対応               | `skos:broader` と OWL のクラス包含は同じ意味ではない。[S-SKOS]     |
| PROV-O / OWL-Time | 出典由来・責任関係 / 時間の概念                    | それ自体が真偽判定やメモリー更新方針を定めるわけではない。[S-PROV] [S-TIME]    |
| JSON-LD           | JSON 構造と RDF/Linked Data 語彙の接続       | 検証やアクセス制御の代替ではない。[S-JSONLD]                       |

RDF 1.2 Concepts の 2026-04-07 Candidate Recommendation Snapshot は、triple term などの拡張を記述する一方、この Snapshot 自体は Recommendation ではないと明示している。実運用では関連仕様・ツールの対応状況を個別に確認する。[S-RDF12]

### OWL の意味論で誤解しやすい点

OWL は開世界の前提を使う。記述が見つからないことだけでは、その命題が偽とはいえない。これは「知らない」と「偽」を区別するために役立つが、欠けた必須フィールドをエラーにしたい用途には別の検証機構が必要になる。[S-OWL]

通常の論理的含意は単調である。公理や事実を足すと新しい含意が増えうるが、既に導けた結論を追加情報だけで自動撤回する信念改訂の仕組みではない。[S-RDF-SEM]

OWL は unique-name assumption（UNA）を前提にしない。異なる IRI が必ず異なる個体を示すとは限らない。オブジェクトプロパティを functional と宣言すると、同じ主語について複数の目的語があっても、それらの個体は同一と推論される。値が一つ存在することを要求する宣言ではない。複数の値を `owl:differentFrom` などで明示的な別個体と宣言した状態で functional とすると、不整合になる。必須値や値の個数を入力時に検査する場合は、SHACL や DB 制約を使う。[S-OWL] [S-SHACL]

`owl:sameAs` は個体の同一性を表す強い関係で、同一個体についての記述が互いに置き換え可能になる。名前が似ている、候補として近い、といった事情だけで付けると、意図しない事実まで結び付けうる。異なる概念体系の用語対応には、個体同一性とは意味の異なる SKOS mapping を検討する。[S-OWL] [S-SKOS]

## 形状検証と問い合わせ

SHACL はデータ graph を shapes graph と照らし、制約に関する検証結果を返す。ここでは人物ごとに少なくとも一つの勤務先関係があり、その値が SHACL のクラス判定で `Organization` に属することを検査する。RDFS/OWL の推論を検証にどう反映するかは、実装・設定に依存するため、例では必要な型をデータに明示している。[S-SHACL]

```turtle
@prefix ex: <https://example.org/memory/> .
@prefix sh: <http://www.w3.org/ns/shacl#> .

ex:PersonShape a sh:NodeShape ;
    sh:targetClass ex:Person ;
    sh:property [
        sh:path ex:worksFor ;
        sh:class ex:Organization ;
        sh:minCount 1
    ] .
```

SPARQL で例の勤務先関係を検索する。

```sparql
PREFIX ex: <https://example.org/memory/>

SELECT ?person ?employer
WHERE {
  ?person a ex:Person ;
          ex:worksFor ?employer .
  ?employer a ex:Organization .
}
```

この query が対象にする graph に推論結果が含まれるかは、ストアの entailment 設定などで変わる。SPARQL の query pattern はデータ上にある対応関係を探すもので、オントロジーの意味論そのものと同一ではない。[S-SPARQL-QUERY]

ここにある Turtle、SHACL、SPARQL のコード例は仕様とコードの静的確認に基づく。RDF パーサ、SHACL 検証器、SPARQL エンジンを使った実行は行っていない。

**問いから利用までの流れ**

- `q` = **答えられるべき問い** — Competency questions
- `v` = **語彙と公理** — クラス・関係・意味
- `d` = **個体と主張** — RDF facts
- `i` = **含意と検証** — OWL/RDFS と SHACL
- `a` = **照会・評価** — SPARQL と用途
- `q` → `v` — 必要な概念を特定
- `v` → `d` — 語彙で記述
- `d` → `i` — 推論・形状検査
- `i` → `a` — 質問に答える

## 答えられるべき問いから設計する

Competency questions は「この知識基盤が答えられるべき問い」を指す。設計を用語の列挙から始めず、想定利用者と問いを明確にし、それを満たす概念・関係・制約を定義する。以下は Noy と McGuinness の反復的な設計手順を、実務向けにまとめたもの。[D-ONTO101]

- • 領域と用途を決める。対象範囲、利用者、答えない範囲を短く記録する。
- • 答えられるべき問いを列挙する。「誰がどの組織に所属するか」「その情報の根拠は何か」など、答えに必要な情報が分かる形にする。
- • 既存語彙の再利用を調べる。外部用語をそのまま使うか、対応関係を定義するか、独自語彙を足すかを決める。
- • 主要なクラスと階層を作る。クラスは種類を表し、特定の人物・組織などは個体として扱う。
- • プロパティを定義する。名前、意味、domain/range、単値・多値、逆関係、時間性を検討する。
- • 必要な公理とデータ制約を分ける。推論したい意味は OWL/RDFS、入力時に満たす条件は SHACL や DB 制約に置く。
- • competency questions を実データで試す。推論器・shape validator・query を使い、答えが出るか、意図しない含意が出ないかを見る。
- • 識別子・版・変更方針を管理する。用語を改名・非推奨化するときの移行方法、データ所有者、変更理由を記録する。

最初から全領域をモデル化しない。問いを満たす最小の語彙を作り、欠けている概念や過剰な公理が見つかったら反復して修正する。評価は「形式的に整っている」だけでなく、予定する利用者と問いに適しているかで行う。[D-ONTO101]

### OWL 2 profile を選ぶ

OWL 2 profiles は表現力と推論上の特性が異なるサブセットである。理論上の計算量特性を、個別データセットの実行速度保証と取り違えない。[S-PROFILES]

| Profile | 主な設計意図            | 選ぶときの考慮                       |
| ------- | ----------------- | ----------------------------- |
| EL      | 大きなクラス・プロパティ階層の分類 | 必要な公理が EL で表現できるか             |
| QL      | 関係データベース上の問い合わせ回答 | DB への query rewriting と必要な表現力 |
| RL      | ルール実行に親和的な推論      | 使用するルールエンジンと許容する OWL 機能       |

必要な含意を具体例で列挙し、それを満たす最小限の profile やルールを選ぶ。OWL の全機能を使うこと自体を目標にしない。

## 語彙の再利用とモデルの限界

既存語彙は相互運用を助けるが、別々の語彙が同じ単語を同じ意味で使うとは限らない。再利用の前に、定義、範囲、識別子、ライセンス、保守状況、必要な推論を確認する。大規模な上位オントロジーや百科事典由来データをすべて取り込む必要はない。

| 資源                             | 用途の例                                     | 選定時の注意                                                          |
| ------------------------------ | ---------------------------------------- | --------------------------------------------------------------- |
| Schema.org                     | Web 上の人物・組織・文書情報                         | 実用的な共有語彙。厳密な業務制約の代替とは限らない。[D-SCHEMA]                            |
| Wikidata                       | statement、qualifier、reference、rank を持つ知識 | rank を校正済みの真偽確率として扱わない。[D-WIKIDATA]                             |
| SKOS                           | taxonomy、thesaurus、概念体系の対応               | `broader` は OWL の `subClassOf` と同じではない。[S-SKOS]                 |
| BFO / DOLCE / SUMO             | 上位概念をそろえる必要のある領域                         | 採用が必要な用途か、既存語彙との対応を確認する。[D-BFO] [D-DOLCE] [D-SUMO]              |
| WordNet / DBpedia / ConceptNet | 語彙関係、百科事典データ、概念連想                        | 語義や抽出データを、個別事実の権威ある根拠とみなさない。[D-WORDNET] [D-DBPEDIA] [D-CONCEPT] |
| OBO Foundry                    | 生物医学分野のオントロジー群                           | 公開性、識別子、文書化、保守などの原則を確認する。[D-OBO]                                |

上位オントロジーは異なる領域間の整合に役立つことがある一方、不要な抽象化や複雑さも招く。まず自分の competency questions に必要な概念から設計する。PROV-O は主張の派生・帰属を表す語彙を提供するが、出典の信頼性や内容の真偽を自動判定するものではない。[S-PROV]

## 文脈・時間を持つ関係

単純な `ex:aoi ex:worksFor ex:aozora-lab` は関係を簡潔に表せるが、その主張の期間、原資料、役割を同じ triple に直接持たない。追加属性を持つ関係をノードとして表す N-ary pattern や assertion pattern を使えば、主語・関係・目的語を保ちながら、期間や根拠を別のプロパティで記録できる。[S-NARY]

```turtle
@prefix ex: <https://example.org/memory/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:claim-1 a ex:EmploymentAssertion ;
    ex:subject ex:aoi ;
    ex:employer ex:aozora-lab ;
    ex:validFrom "2026-04-01"^^xsd:date ;
    ex:knownFrom "2026-04-10T09:00:00+09:00"^^xsd:dateTime ;
    prov:wasDerivedFrom ex:message-1 .
```

このカスタム例では、有効開始日と受領時刻を別々のプロパティとして表した。どちらが実世界での時間でどちらがシステムに入った時間か、終了日をどう扱うか、訂正前の主張を残すかは、アプリケーションの契約で定める。OWL-Time は時間点・区間などを語彙化するが、履歴保持・競合解決の方針は別途必要になる。[S-TIME]

関係をノードにする設計は表現力を増すが、単純な二項関係より検索や制約が複雑になる。値・根拠・時刻などを後で照会する必要がある場合に採用し、直接 triple で十分な関係は簡潔に保つ。具体的なメモリーの取得、訂正、撤回、評価への適用は[オントロジーとメモリーの応用例](ontology-memory-applications.md)を参照。

## LLM とオントロジー

LLM を使ったオントロジー学習では、用語の型付け、分類階層の発見、分類以外の関係抽出などを分けて評価する研究がある。[P-LLMOL] これは出力をそのまま採用できることを意味しない。生成されたクラスや関係について、定義の重複、粒度、既存用語との整合性、問いへの有用性を人または検証器が確認する。

LLM を既存語彙に ground する場合、表記から候補の識別子を探す処理と、その候補が文脈上正しい意味か判断する処理を区別する。OntoGPT の運用文書は、抽出後の用語について語彙中の存在やラベルを確認する手順を示すが、識別子の存在確認だけで文脈的な意味の正しさが保証されるわけではない。[D-ONTOGPT] [D-ONTOCHECK]

実務では、LLM の出力を候補として扱い、識別子・型・語彙バージョン・根拠範囲を記録し、SHACL 等で形状を検証し、competency questions の query で有用性を試す。抽出品質とオントロジー設計品質は別の評価対象である。LLM を使う範囲と評価指標は、[関連する応用編](ontology-memory-applications.md)にもまとめている。

## 既存調査からたどる応用研究とツール

この節は 2026-09-28 の調査記録にあった研究・ツール紹介を引き継いだもので、今回新たに再調査した内容ではない。

2026 年の challenge 対応研究には、既存語彙へ候補を制約する方式もある。自然な関係の生成と語彙への整合は別に評価する。[P-LLMOL26]

AutoSchemaKG は entity・event の抽出に加え、概念化から schema を誘導する研究である。未知領域から構造を作る方向を示すが、業務上の問いや制約を満たすかは追加検証が必要になる。[P-AUTOSCHEMA]

Protégé はオントロジー編集の環境、ROBOT はオントロジー処理の自動化に使うツールとして調査記録に挙げられている。[D-PROTEGE] [D-ROBOT]

Cyc の microtheory は、文脈に応じた知識を分ける設計例として記録されている。歴史的な OpenCyc と現在の Cycorp 製品は同一視しない。[D-CYC] Princeton WordNet の公式ページについては、2026-09-28 の確認時に新規開発の停止とコミュニティ資源への案内が記載されていた。追加確認の範囲は資料台帳に分けて記録している。[D-WORDNET]

## 関連ドキュメント

- [知識・メモリーの基本概念](concepts.md)
- [オントロジーとメモリーを組み合わせる応用例](ontology-memory-applications.md)
- [知識更新の理論と実装](knowledge-updates.md)
- [グラフ・推論基盤の選択肢](../implementation/infrastructure.md)
- [オントロジー・ナレッジシステムの推奨設計](../../design/ontology-knowledge-system.md)

[D-GRUBER]: https://tomgruber.org/writing/ontolingua-kaj-1993/

[D-ONTO101]: https://protege.stanford.edu/publications/ontology_development/ontology101-noy-mcguinness.html

[S-RDF]: https://www.w3.org/TR/rdf11-concepts/

[S-RDFS]: https://www.w3.org/TR/rdf-schema/

[S-OWL]: https://www.w3.org/TR/owl2-primer/

[S-RDF-SEM]: https://www.w3.org/TR/rdf11-mt/

[S-SHACL]: https://www.w3.org/TR/shacl/

[S-SPARQL-QUERY]: https://www.w3.org/TR/sparql11-query/

[S-SKOS]: https://www.w3.org/TR/skos-reference/

[S-PROV]: https://www.w3.org/TR/prov-o/

[S-TIME]: https://www.w3.org/TR/owl-time/

[S-JSONLD]: https://www.w3.org/TR/json-ld11/

[S-PROFILES]: https://www.w3.org/TR/owl2-profiles/

[S-NARY]: https://www.w3.org/TR/swbp-n-aryRelations/

[S-RDF12]: https://www.w3.org/TR/2026/CR-rdf12-concepts-20260407/

[D-SCHEMA]: https://schema.org/docs/datamodel.html

[D-WIKIDATA]: https://www.wikidata.org/wiki/Help:Data_model

[D-BFO]: https://github.com/BFO-ontology/BFO-2020

[D-DOLCE]: https://www.loa.istc.cnr.it/index.php/dolce/

[D-SUMO]: https://www.ontologyportal.org/

[D-WORDNET]: https://wordnet.princeton.edu/

[D-DBPEDIA]: https://www.dbpedia.org/resources/ontology/

[D-CONCEPT]: https://conceptnet.io/

[D-OBO]: https://obofoundry.org/principles/fp-000-summary.html

[P-LLMOL]: https://arxiv.org/abs/2307.16648v2

[D-ONTOGPT]: https://github.com/monarch-initiative/ontogpt/blob/main/docs/custom.md

[D-ONTOCHECK]: https://github.com/monarch-initiative/ontogpt/blob/main/docs/operation.md

[D-CYC]: https://cyc.com/wp-content/uploads/2021/04/Cyc-Technology-Overview.pdf

[P-LLMOL26]: https://arxiv.org/abs/2608.27101v2

[P-AUTOSCHEMA]: https://arxiv.org/abs/2505.23628v3

[D-PROTEGE]: https://protege.stanford.edu/software/

[D-ROBOT]: https://github.com/ontodev/robot
