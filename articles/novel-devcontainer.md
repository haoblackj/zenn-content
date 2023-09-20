---
title: "Dev Containerで小説を開発する"
emoji: "🛠️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - GitHub
  - VSCode
  - devcontainer
  - Tech
published: false
---
# 要点
- Dev Container を使って小説を書く
- プロファイルと組み合わせて最強の執筆環境を作ろう
# Dev Container を使って小説を書く
前にこのような記事を投稿した。
https://zenn.dev/haoblackj/articles/novel-codespaces
GitHub Codespaces^[GitHub が提供するクラウド上の VSCode 環境]を使って、端末を問わずに小説を執筆できるようにするというものだ。
しかし、この Codespaces という機能には、以下のような制約がある。
- 利用時間に制限がある
  - 1 か月に 120 時間まで
  - GitHub Pro ユーザなら 1 か月に 180 時間
- オフライン環境では使えない
  - クラウド上の環境なので当然
- 非アクティブなままの Codespaces は自動で削除されてしまう
  - プッシュしていない変更があっても削除されてしまう
  - デフォルトでは 1 か月に 30 日以上非アクティブなままだと削除される

費用面については公式情報を参照されたい。
https://docs.github.com/ja/billing/managing-billing-for-github-codespaces/about-billing-for-github-codespaces

クラウド上の環境に接続して執筆するのだから、上記はどれも当然の制約だ。
とはいえ、私としてはそれでは困る。
なので、Dev Container を使うことにしたのだ。

## Dev Container とは
