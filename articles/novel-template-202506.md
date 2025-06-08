---
title: "小説執筆リポジトリの今"
emoji: "✒"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["GitHub","VSCode","VSCode拡張機能","textlint","Tech"]
published: true
---
# 要点
- 前記事からの変更点
  - `vivliostyle` の手動インストールが不要になった
  - novel-builder の復活
  - textlint 拡張機能の更新

# 小説の執筆環境を整理した
## 環境構築
以下のテンプレートリポジトリを使えば、諸々設定済みの環境が立ち上がる。
https://github.com/haoblackj/Novel-Template
リポジトリ作成方法・リポジトリ作成後の必要作業については、上記テンプレートリポジトリの README.md に記載している。
みんな使ってね。

## 前記事からの変更点
### vivliostyle の手動インストールが不要になった
前記事では、`vivliostyle` の手動インストールが必要だった。
VSCode で小説を書くにあたって事実上必須であるところの `novel-writer` の一部機能が、`vivliostyle` を必要としていたためである。
https://marketplace.visualstudio.com/items?itemName=TaiyoFujii.novel-writer
しかし 2025 年 6 月現在、`novel-writer` 自体に `vivliostyle` が含まれている。
そのため、`vivliostyle` の手動インストールは不要になった。

なお、`vivliostyle` のインストールはリポジトリのセットアップとは別の dotfiles にて行っていたため、テンプレートリポジトリには反映されていない。

:::message alert
`novel-writer` の機能に `vivliostyle` が含まれているのは、バージョン 3.2.0 以降である。
また、PDF 出力には依然として `Node.js` のインストールが必要である。
記事冒頭のテンプレートリポジトリも、`Node.js` がインストールされていることを前提にしている。
:::

### novel-builder の復活
前記事で `novel-builder` の廃止と書いた。
>`novel-builder` は、各小説サイトの記法に沿って原稿ファイルを整形・出力してくれるツールである。
>しかし、元から投稿先の小説投稿サイトに合わせた記法で書けばいいという発想に至り、このツールを使う必要性を感じなくなった。

しかしその後、身内向けの添削用プレプリントサーバーを立てることになり、`novel-builder` の必要性を感じた。
`Cloudflare Pages` 上に `Hugo` でデプロイするのだが、その過程で `txt` を `Markdown` に変換しつつ `Front Matter` を付与する必要があった。
また、ハーメルン形式のルビ付与を HTML に変換するなどの、シェルスクリプトで行うには面倒な処理も必要だった。
そのため、`novel-builder` を復活させた。

https://github.com/8amjp/novel-builder

ただし、原稿の格納ディレクトリは、`novel-builder` のデフォルト値とは異なる。
`novel-builder` のデフォルト値は `./episodes` だが、テンプレートリポジトリでは `./draft` としている。

```package.json
  "config": {
    "episodes_dir": "draft"
  },
```

これは、`novel-writer` のデフォルト値と合わせるためである。

プレプリントサーバーへのデプロイについては、今後あらためて記事にする予定だ。

### textlint 拡張機能の更新
前記事では、`textlint` の VSCode 拡張機能を `vscode-textlint` としていた。
ただ、`vscode-textlint` はメンテナンスされておらず、最新の `textlint` と衝突する状態が続いていた。
現在、当該拡張機能は `textlint` となり、`textlint` 開発コミュニティによってメンテナンスされている。
https://marketplace.visualstudio.com/items?itemName=3w36zj6.textlint
そのため、テンプレートリポジトリでも `textlint` 拡張機能を使用するように変更した。

拡張機能の開発元移行については、下記も参照のこと。
https://github.com/orgs/textlint/discussions/2

これまでの開発とメンテナンスに感謝を。

# まとめ
昨年の断捨離記事からの変更点をまとめた。
周辺事情によって機能追加・削減があったものの、環境に求める機能や勝手が固まってきた印象がある。
とはいえ、小説執筆環境は盆栽であり、改善には喜びが伴う。
これからも、引き続きちまちまと手入れを重ねていくつもりだ。