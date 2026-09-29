# 追加候補と調査範囲の境界

[調査トップ](../README.md) / [システム比較](README.md) / [資料台帳](../sources.md)

このページは README・manifest を中心に確認した候補。主要システムの詳細ページと同じ深さのアルゴリズム監査は行っていない。提供者の説明と、次に検証すべき問いを示す。

**追加確認:** 2026-09-29 の[メモリーと経験の再調査](memory-landscape.md)で、ここに挙げた候補を再訪し、代表実装の更新・検索経路を深掘りした。以下は初回の確認範囲を残した概要で、追加の根拠・確認の深さは新しい詳細ページを参照する。

| 候補 | 確認した中心設計 | 参考になる点 | 次に確認する制約 |
|---|---|---|---|
| MemoryOS（BAI-LAB） | short / mid / long-term personal memory の階層。FIFO とページ構成を使う昇格 | 頻繁に使う文脈を保持し、履歴を階層化する | MemTensor/MemOS と別。昇格で失われる条件、根拠、古い知識の抑止。[P-MEMORYOS] [D-MEMORYOS] |
| MemMachine | episodic の graph、profile の SQL、working memory | 種類ごとに異なる保存・検索を組み合わせる | ストア間の整合性、profile と episode が競合した時の規則。[D-MACHINE] |
| memU | agent が作る Markdown/skill を蓄積し、検索する現行構成 | 人が読める記憶とホスト側 agent による抽出の分離 | 現行 README の MemoryService は LLM/chat call を行わない。過去の紹介の自動抽出方式と混ぜない。抽出品質はホスト側にも依存。[D-MEMU] |
| OpenViking | `viking://` の仮想 filesystem に memory / resource / skill、L0 abstract / L1 overview / L2 detail | 抽象度を選んで取り出し、検索経路を観察する | VikingMem 論文の全機能が公開版にあるとは限らない。各層の更新・削除伝播を検査。[D-OPENV] [P-VIKING] |
| MIRIX | Core / Episodic / Semantic / Procedural / Resource / Knowledge Vault | マルチモーダル経験と記憶の役割分担 | 今回は論文要旨まで。画像・音声の抽出誤り、記憶型間の矛盾、データ量の検証が必要。[P-MIRIX] |

## 短い紹介だけで選ばないための手順

1. 同名 package、論文実装、ホスト型サービスを識別する。
2. 公開 README が指す entry point と実際の manifest を確認する。
3. 一つの事実が追加・訂正・撤回される経路をコード上でたどる。
4. 保存される根拠と、取得できる履歴を API とデータモデルで確認する。
5. 書き込みと回答の両方を、[共通評価](../evaluation.md)へ接続する。

今回の実装調査では、SimpleMem のように一つの repo に text・multimodal・別方式が同居する例、Letta のように開発先が移動する例、Mem0 のように本文コメントと処理が違う例があった。導入判断では repo 名や古いブログだけを記録せず、コミットと entry point をセットにする。

## 関連ドキュメント

- [調査方法と未確認範囲](../methodology.md)
- [実装の依存関係](../implementation/dependencies.md)
- [共通の評価方法](../evaluation.md)

[P-MEMORYOS]: https://arxiv.org/abs/2506.06326v1
[D-MEMORYOS]: https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/README.md
[D-MACHINE]: https://github.com/MemMachine/MemMachine/blob/d57f5cb36a357c01085f571a0cdd2dfbc9882f89/README.md
[D-MEMU]: https://github.com/NevaMind-AI/memU/blob/2c050bc9681a4c0aff1af211a000e73d14f33356/README.md
[D-OPENV]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/README.md
[P-VIKING]: https://arxiv.org/abs/2605.29640v3
[P-MIRIX]: https://arxiv.org/abs/2507.07957v1
