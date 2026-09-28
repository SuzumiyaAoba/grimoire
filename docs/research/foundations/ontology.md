# オントロジーと知識表現

[調査トップ](../README.md) / [資料台帳](../sources.md)

## まず、必要な意味を決める

今回必要なのは、人名や関係名の一覧に加えて、主張の対象、意味、適用条件、根拠、変更可能性を共有する仕組みである。型を使う場所には、抽出プロンプト、保存時の検証、問い合わせ時の推論、保持・失効方針という四つがある。同じ ontology を使っていても、どの機能に使うかで結果は異なる。

| 層 | 具体例 | 役割 |
|---|---|---|
| 語彙 | `Person`, `Organization`, `worksFor` | 名前と意味を揃える |
| TBox / RBox | Employee は Person、関係の domain/range | 型と関係の公理 |
| ABox | Alice は Employee、Alice worksFor Acme | 個体に関する主張 |
| shape | Assertion には source が最低一つ必要 | 保存したデータの検証 |
| lifecycle policy | 現住所は置換、出生地は通常不変 | 知識更新のアプリ規則 |

TBox/ABox の区別は説明のためで、別ファイルや別 DB が必須という意味ではない。OWL は用語的・個体的な公理をどちらも記述できる。[S-OWL]

## 標準の役割と限界

| 標準 | 得られるもの | 得られないもの |
|---|---|---|
| RDF 1.1 | IRI、literal、triple、dataset / named graph | グラフ名だけからの出典・時刻の意味づけ。[S-RDF] |
| RDFS / OWL 2 | クラス、関係、公理、形式的含意 | ソースに存在しない必須値の入力検査を通常の DB と同じように行うこと。[S-OWL] |
| SHACL | shape、datatype、cardinality、path、閉じた shape 等の検証 | 現実世界での主張の正しさ。[S-SHACL] |
| SKOS | 概念、preferred/alternative label、broader/narrower、mapping | 任意の実体間の完全な同一性。[S-SKOS] |
| PROV-O | Entity / Activity / Agent と派生・帰属の関係 | 引用先の信頼性の自動認定。[S-PROV] |
| OWL-Time | 時点、区間、前後・重なり等の時間関係 | DB の全 revision の保存や、更新時の自動訂正。[S-TIME] |
| SPARQL | RDF pattern、path、集約、更新 | LLM による質問→query 変換の正確性。[S-SPARQL] |
| JSON-LD | JSON と Linked Data の識別子・語彙の接続 | JSON Schema や権限制御の代替。[S-JSONLD] |

### 開世界・単調性・同一性

OWL の開世界の考え方では、記載がないだけでは偽と判定しない。通常の含意は単調で、公理を追加して既存の含意を自動的に取り消す信念改訂とは異なる。一般に unique-name assumption も置かないため、異なる IRI の二人が必ず別人だとは限らない。functional property の多値が、期待する「入力エラー」でなく個体の同一性の帰結に関わる場合もある。アプリの制約は SHACL や DB 制約として明示する。[S-OWL] [S-SHACL]

`owl:sameAs` は強い同一性であり、文字列や embedding が似ているという理由だけで付けない。候補の別名、同一実体の仮説、確定した同一性を別に保存する。概念間の緩い対応には SKOS mapping などを検討するが、その意味の違いを保つ。[S-OWL] [S-SKOS]

### OWL profile を選ぶ

| profile | 適性 | トレードオフ |
|---|---|---|
| EL | 大きなクラス体系の分類 | 許される表現を制限 |
| QL | データ量の大きい問い合わせ、DB への書き換え | 表現可能な公理が限定 |
| RL | ルールを用いる実装と大規模データ | OWL 全体の表現力は持たない |

これらは用途に応じて計算特性を扱いやすくした profile で、すべての問い合わせが同じ速度になる保証ではない。最初に必要な含意を列挙し、それを満たす最小の profile / rule set を選ぶ。[S-PROFILES]

## 主張への注釈と多項関係

「Alice が Acme に、2025 年から技術顧問として、契約 X に基づき勤務」は一つの無条件な二項関係だけでは表しにくい。雇用イベントや Assertion をノードにし、参加者・役割・期間・契約・根拠を付ける N-ary pattern が使える。[S-NARY]

以下は本調査の例示で、標準語彙を独自の Claim schema へ接続する形である。

```turtle
@prefix ex: <https://example.org/memory/> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

ex:claim-42 a ex:Assertion ;
  ex:subject ex:alice ;
  ex:predicate ex:worksFor ;
  ex:object ex:acme ;
  ex:validFrom "2025-04-01"^^xsd:date ;
  ex:status ex:supported ;
  prov:wasDerivedFrom ex:message-19 ;
  prov:wasAttributedTo ex:alice .
```

この形式では、`ex:alice ex:worksFor ex:acme` を無条件に assert したことにはしない。採用済みで権限・時点に合う Assertion から、回答用 graph を導出する方針にできる。RDF reification、named graph、RDF 1.2 triple term は別の表現方法であり、互換性と意味論を確認して選ぶ。[S-RDF]

RDF 1.2 Concepts の今回確認した版は **2026-04-07 Candidate Recommendation Snapshot**。triple term / reifier の表現を検討できるが、RDF-star の旧構文・語彙と混ぜない。関連 SPARQL・serializer・validator の対応も個別に確認する。[S-RDF12]

## 再利用できる既存語彙・知識

| 資源 | 主な性質 | 今回の利用方法と限界 |
|---|---|---|
| Schema.org | Web 上の人物、組織、文書、行動等の実用語彙 | 外部データとの交換。厳密な業務制約は別途定義。[D-SCHEMA] |
| Wikidata | statement に qualifier・reference・rank | 時間・出典・複数値の実例。rank は校正された真実確率ではない。[D-WIKIDATA] |
| BFO | 科学的な上位分類 | 物・過程等を厳密に揃える必要がある領域。全サービスへの導入は必須でない。[D-BFO] |
| DOLCE | 人間の言語・認知を意識した基礎オントロジー | 出来事・性質・社会的対象の整理。FOL と OWL 版の表現範囲の差に注意。[D-DOLCE] |
| SUMO | 広い上位語彙と論理的定義 | 共通知識への接続。SUO-KIF 等の表現・推論環境を確認。[D-SUMO] |
| Cyc | 常識・公理と microtheory による文脈 | すべてを一つの大域的真実へまとめない設計の参考。現行製品と歴史的 OpenCyc を同一視しない。[D-CYC] |
| ConceptNet | 多言語の概念・常識関係 | 検索語の拡張や候補生成。特定人物の現在の事実を確定する出典にはしない。[D-CONCEPT] |

DBpedia、WordNet、OBO Foundry の領域オントロジーも接続候補になる。百科事典からの抽出、語義辞書、分野の専門語彙という役割の違いを認識し、必要な領域・言語に絞って選ぶ。全資源を初期ロードすると、同定・更新・ライセンスの管理対象が増える。[D-DBPEDIA] [D-WORDNET] [D-OBO]

Princeton WordNet の公式ページは新規開発の停止とコミュニティ資源への案内を記載している。語彙そのものの価値と保守状況を別に評価する。[D-WORDNET]

## オントロジーを LLM で作る・利用する研究

**LLMs4OL** は term typing、taxonomy discovery、非分類関係の抽出を分けて評価する。2026 年の challenge 対応研究には既存語彙へ候補を制約する方式もあり、「もっともらしい関係を生成した」だけでは既存 ontology に整合したことにならない。[P-LLMOL] [P-LLMOL26]

**AutoSchemaKG** は entity と event の抽出に加え、概念化から schema を誘導する。ドメインが未知の入力を広く処理する方向として重要。ただし生成した分類が、特定業務の competency questions、制約、版互換性を満たすかは別途検証する。[P-AUTOSCHEMA]

**OntoGPT / LinkML** は構造化 template と既存用語への grounding を接続する。運用文書は抽出後に識別子を term validator で検査することを説明する。識別子が存在する検査と、その文の対象に正しく対応した検査は別である。[D-ONTOGPT] [D-ONTOCHECK]

更新する ontology 自体にも版、変更理由、deprecated term の置換先、既存データの migration が必要。Protégé は編集・共同作業、ROBOT は自動化の入口になる。[D-PROTEGE] [D-ROBOT]

## 関連ドキュメント

- [オントロジー・ナレッジシステムの推奨設計](../../design/ontology-knowledge-system.md)
- [メモリーと知識の基本概念](concepts.md)
- [知識更新の理論と実装](knowledge-updates.md)
- [グラフ・推論基盤の選択肢](../implementation/infrastructure.md)

[S-OWL]: https://www.w3.org/TR/owl2-primer/
[S-RDF]: https://www.w3.org/TR/rdf11-concepts/
[S-SHACL]: https://www.w3.org/TR/shacl/
[S-SKOS]: https://www.w3.org/TR/skos-reference/
[S-PROV]: https://www.w3.org/TR/prov-o/
[S-TIME]: https://www.w3.org/TR/owl-time/
[S-SPARQL]: https://www.w3.org/TR/sparql11-overview/
[S-JSONLD]: https://www.w3.org/TR/json-ld11/
[S-PROFILES]: https://www.w3.org/TR/owl2-profiles/
[S-NARY]: https://www.w3.org/TR/swbp-n-aryRelations/
[S-RDF12]: https://www.w3.org/TR/2026/CR-rdf12-concepts-20260407/
[D-SCHEMA]: https://schema.org/docs/datamodel.html
[D-WIKIDATA]: https://www.wikidata.org/wiki/Help:Data_model
[D-BFO]: https://github.com/BFO-ontology/BFO-2020
[D-DOLCE]: https://www.loa.istc.cnr.it/index.php/dolce/
[D-SUMO]: https://www.ontologyportal.org/
[D-CYC]: https://cyc.com/wp-content/uploads/2021/04/Cyc-Technology-Overview.pdf
[D-CONCEPT]: https://conceptnet.io/
[D-DBPEDIA]: https://www.dbpedia.org/resources/ontology/
[D-WORDNET]: https://wordnet.princeton.edu/
[D-OBO]: https://obofoundry.org/principles/fp-000-summary.html
[P-LLMOL]: https://arxiv.org/abs/2307.16648v2
[P-LLMOL26]: https://arxiv.org/abs/2608.27101v2
[P-AUTOSCHEMA]: https://arxiv.org/abs/2505.23628v3
[D-ONTOGPT]: https://github.com/monarch-initiative/ontogpt/blob/main/docs/custom.md
[D-ONTOCHECK]: https://github.com/monarch-initiative/ontogpt/blob/main/docs/operation.md
[D-PROTEGE]: https://protege.stanford.edu/software/
[D-ROBOT]: https://github.com/ontodev/robot
