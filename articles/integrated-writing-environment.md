---
title: "小説の執筆環境で断舎離してみた"
emoji: "🖋"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["GitHub","VSCode","VSCode拡張機能","textlint","Tech"]
published: false
---
# 要点
- 前記事の執筆時点からいろいろと環境が変わった
  - VSCode のプロファイル機能を活用するようになった
  - タスク管理に Issue を使うのをやめた
  - その他、テンプレートリポジトリの修正点紹介

# 小説の執筆環境を整理した
## 環境構築
以下のテンプレートリポジトリを使えば、諸々設定済みの環境が立ち上がる。
https://github.com/haoblackj/Novel-Template
リポジトリ作成方法・リポジトリ作成後の必要作業については、上記テンプレートリポジトリの README.md に記載している。
みんな使ってね。

## 前記事からの変更点
### 新しい記事にした経緯
早いもので、前記事の初稿執筆から既に 2 年近く経過している。
https://zenn.dev/haoblackj/articles/8cbadb26ca16e4
その間にも、ちまちまと小説は書き続けてきた。
当然、「もう少しどうにかならんのか」という文句も(主に自分の中から)出てくるもので、それに合わせてカスタマイズを重ねてはいた。

今回、某所で「VSCode で小説を書くには何が必要か」という話題になり、つい前記事を携えて首を突っ込んだところ、前記事と現在の環境に思ったより多くの差分が出ていたことがわかった。
わかっていて放置するのも忍びないので、こうして記事を書くことにしたというのが、経緯である。

### VSCode の設定関連
まずは、VSCode の設定関連から。
といっても、設定の内容については深入りしない。
今回取り上げるのは、設定の保管先である。

前回の記事では、VSCode の設定はリポジトリ内に持っていた。
`.vscode` ディレクトリ内に `settings.json` を配置し、設定変更時には設定画面の『ワークスペース』から変更していたものである。
リポジトリごとに設定を厳密にコントロールするには、この方法が適していると思われる。

しかし、こと小説執筆に関しては、設定をリポジトリ内に持つ必要性が薄いとわかった。
SF 小説がラブコメ小説でも、誤字脱字等を確認するための PDF の様式は同じである。
アクション小説とミステリ小説で、Textlint のトリガータイミングを変えることはない。
小説執筆に必要な設定はある程度共通であり、あるリポジトリで変更した設定は他のリポジトリでも適用することになる。

リポジトリで管理する必要はないじゃないか、という発想に至るのに、そう時間はかからなかった。
VSCode 自体に小説執筆向けの設定を持たせ、GitHub アカウントを使って同期すればいい。
問題は、前記事執筆時からしばらく、それを実現する方法がなかったことだ。

https://zenn.dev/haoblackj/articles/novel-codespaces
この記事でも述べているように、私は VSCode でいろいろなことをしている。
小説を書き、Zenn で技術記事を書き、Markdown で議事録を取っている。
それらの作業ごとに、違う拡張機能や設定、テーマを用いている。
小説用と Zenn 用の設定は明確に分ける必要があったので、苦肉の策として、リポジトリごとに設定を持つようにしていたのだ。

しかし、今の統合執筆環境ユーザには、プロファイル機能という強い味方がいる。
プロファイル機能を使えば、VSCode の設定を GitHub ユーザに同期できる(Microsoft アカウントを使う手もある)。
小説用プロファイルに小説用の設定を、Zenn 用プロファイルに Zenn 用の設定を、という具合に、プロファイルごとに設定を持たせることができる。
これこそ、私の求めていたものだった。

今、各リポジトリからは `.vscode` ディレクトリが消去されている。
リポジトリをローカルにクローンして、VSCode で開き、プロファイルを選択するだけで、小説執筆に必要な設定がすべて揃うからだ。


### Issue の使用停止
前記事時点では、タスク管理に Issue を使っていた。
思いついたことや今後の展望を Issue に書き、それをタスクとしてこなしていた。

しかし、小説においてのタスク管理は、現在進行中の原稿に深く結びつくものである。
そして原稿は、ソースコードのように、関数やサブルーチンという形で構造化されているわけではない。
時に、今書いている内容を、未だ書いていない数百行後の内容と結びつける必要が生じる。
そのとき、Issue に記録しておいたのでは、かえって一覧性を損なうのだとわかった。

そこで、Issue の使用を停止し、代わりに原稿内へ直接コメントを書くことにした。
タスクが生じたら、コメントとして記載する。
タスクが完了したら、完了済としての記号をコメント内の先頭に打っておく。
仮に一覧として見たいときは、コメントとその位置を抽出するスクリプトを実行すればいい。

小説のタスク管理でカンバン方式を使う必要性は、少なくとも私には、全くなかった。
(もちろん、カンバン方式を使うほうが捗るという人もいるだろう。私も試すまでわからなかった)

### テンプレートリポジトリの修正点
他、テンプレートリポジトリに加えた修正点を、ざっと挙げておく。
- `novel-writer` の一部機能を使えるよう、セットアップスクリプトを修正。
  - `novel-writer` は、小説執筆に便利な機能を提供する VSCode の拡張機能である。
  - この中に、原稿内容を PDF で確認するための PDF をプレビュー・出力する機能があり、これを頻繁に使うようになった。
  - そのため、セットアップスクリプトにこの機能を最初から使えるようにするための処理を追加した。
    - 具体的には、`@vivliostyle/cli` というライブラリを `npm` でインストールする処理を追加している。
    - `packages.json` に追加しないのは、拡張機能側でグローバルへのインストールを推奨されているため。
- `novel-builder` の廃止。
  - `novel-builder` は、各小説サイトの記法に沿って原稿ファイルを整形・出力してくれるツールである。
  - しかし、元から投稿先の小説投稿サイトに合わせた記法で書けばいいという発想に至り、このツールを使う必要性を感じなくなった。
    - 複数の小説投稿サイト向けに書く場合は、このツールを使うことで、それぞれの記法に合わせた原稿を出力できる。私のニーズには合わないが、便利なツールであることは確か。
- `devcontainer` の廃止。
  - VSCode のプロファイル機能を使うため、`devcontainer` も不要となった。
- Issue 制御関連の GitHub Actions を廃止。
  - Issue の使用を停止したため、それに関連する GitHub Actions も不要となった。


# まとめ
およそ 2 年ほど環境を使い続けて出てきた変更点を、一挙にまとめた。
経緯を棚卸ししてみると、自分が小説執筆に求めることの変化がよくわかる。
今後も、ちまちまと手入れを続けることになるだろう。

原稿は代わり映えしないのにな。