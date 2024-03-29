---
title: GitHub Codespaces で小説を書く時代
emoji: 📌
type: tech
topics:
  - GitHub
  - VSCode
  - codespaces
  - Tech
published: true
published_at: 2022-08-22 14:40
---

# 概要
- 執筆環境は「使い回す」時代へ

# 執筆環境について
以前、こんな記事を書いた。
https://zenn.dev/haoblackj/articles/8cbadb26ca16e4
ありがたいことに多くの反響をいただいている。
しかし、筆者自身は上の記事で紹介したメソッドに少なからぬ不満がある。
##  端末の数だけ環境がある
上の方法では、小説を書く環境を端末ごとにセットアップしなければならない。
VSCode に拡張機能をインストールし、リポジトリをクローンしてきて、```npm ci``` だの ```yarn install``` だのを叩かなければならない。
なるほど VSCode には Setting Sync^[設定やショートカット、拡張機能を同期してくれる機能]があるし、リポジトリのクローンも Git のエイリアスや GitHub CLI を使えば面倒がない。投入すべきコマンドなどは ```README.md``` にでもメモしておけばいい。
込み入った話だが、リポジトリごとに **.vscode** ディレクトリを作成して、インストールすべき拡張機能を制御することも出来る。
しかし、これらの方法には以下の問題点がある。
### 拡張機能の制御
筆者はリポジトリごとに使う拡張機能を決めている。

小説を Markdown 形式で書いてはいるが、**novel-writer** を用いている以上、言語モードは **novel** にしてくれないと困る。
一方、Zenn 執筆用のリポジトリでは、Markdown 形式のファイルは Markdown として認識してくれないと困る。**VS Code Zenn Editor** は Markdown 言語モードでないとプレビューどころか認識さえしてくれないからだ。

残念ながら、VSCode の Setting Sync はリポジトリやワークスペースごとの拡張機能制御に対応していない。

### 環境設定ファイルの制御
VSCode だけでなく、Git 自体にも設定項目がある。
- autoCRLF … OS に応じて改行コードを変換してくれる機能(True/False)
- alias … コマンドを自分なりに短縮して登録できる機能

上記は .gitconfig ファイルに登録されるのだが、これも端末ごとの差異要因になる。
さりとて Setting Sync のような自動同期機能は内包していないので、ひとつ修正するごとに他の端末でも修正しないとならない。
リポジトリに設定ファイルを格納できるが、そうすると今度はリポジトリごとに設定ファイルを修正する必要が出てくる。

GitHub CLI という GitHub 謹製のコマンドラインツールにも設定ファイルがあるのだが、こちらはそもそもリポジトリに設定ファイルを格納できない。

残念ながら、VSCode はこういった設定項目を直接同期する機能がない。
VSCode の設定項目ではないのだから当然だ。

### リポジトリの制御
まずリポジトリをローカルに落としている時点で差異が発生する。
未だに Pull のし忘れでマージコミットを発行することになったりする。
端末ごとにローカル環境があれば、差異が生じるのも当然だ。

## 環境をひとつに
GitHub Codespaces が上記の問題をほぼ解決してくれた。

- 拡張機能
設定ファイルに拡張機能 ID を記述するだけでいい。
(Setting Sync の拡張機能同期はあらかじめ無効にしておく)
- 環境設定ファイルの制御
`setting.json` を作成し、`.vscode` ディレクトリの中に格納することで同期できる。
- リポジトリの制御
GitHub 上のコンテナに都度アクセスするような形なので、差異の生じる危険性が低減できる。

早い話が、GitHub 上にローカルリポジトリも置いて、端末を問わず使い回すという構図だ。
Google Chrome などの Web ブラウザでも、VSCode とほぼ同じ見た目の画面でテキストを打ち込める。
iPad や Android タブレットでもまともに小説を書ける時代が来たのだ。

具体的な使用手順、カスタマイズ方法は下を参考にしてほしい。
https://zenn.dev/dzeyelid/articles/5485cdeb2a1ada
https://zenn.dev/dzeyelid/articles/a30a98618c40bd

現在、小説執筆テンプレートリポジトリを Codespaces へ対応させるべく修正作業を行っている。
https://github.com/haoblackj/Novel-Template

小説向けの Codespaces 設計などは、また別の記事で詳しく書くことにしたい。

### P.S.
今回の記事は、下ツイートの求めに応じて執筆された。
https://twitter.com/haoblackj/status/1561523605548122112

ぶっちゃけ下書きのようなものである。

## 追記(2024年1月17日)
VSCode にプロファイル機能が実装され、リポジトリごとの拡張機能制御が可能になった。
プロファイルは Setting Sync で同期できる。
左下の歯車マークからプロファイルを選択するだけで、設定と拡張機能が切り替わり、必要に応じて拡張機能が出し入れされる。
Codespaces は依然として有用だが、VSCode でもリポジトリごとの環境を作れるようになったのは大きい。

# 最後に
この記事は、下の GitHub Actions を用いて予約投稿している。
https://github.com/x-color/zenn-post-scheduler

説明記事はこちら。
https://zenn.dev/x_color/articles/create-zenn-post-scheduler

あなたも使おう。