---
title: GitHub Codespaces で小説を書く時代(詳細版)
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics:
  - GitHub
  - VSCode
  - codespaces
  - Tech
published: false
published_at: 2022-12-29 14:40
---

# 今回の記事について
この記事は、以下の記事の続編的な位置づけである。
https://zenn.dev/haoblackj/articles/novel-codespaces
できるだけこの記事単独で理解できるように説明をするが、ご一読いただいたほうが理解は早いと思われる。

小説執筆環境については、以下の 2 記事を参照されたい。
https://zenn.dev/haoblackj/articles/8cbadb26ca16e4
https://zenn.dev/haoblackj/articles/manuscript_compare_by_pr

# 執筆環境の抱える問題
なーに書いたらいいんだかぜーんぜんわからん、何人だよ顔ひらたすぎだろ我が偉大なる祖国の名くらい知っておろうローマ。
要は Settings Sync で同期する問題は余計な拡張機能が流れ込んでくることであり、キーマップやら言語モードやらハイライトやらを上書きしてくれちゃうこと。
拡張機能の部分を同期から外して、拡張機能は devcontainer.json に記述すれば丸く収まる。
↑今年になって VSCode 標準機能に「Profile」というのが入って、これがなかなかゲイコマ。設定と拡張機能とキーバインドをプロファイルという形で複数管理できる。ディレクトリ単位で選択したプロファイルを記憶してくれるので、いちいち選択する必要もない。Setting Sync 経由でプロファイルが同期されるから、新しい PC に VSCode インストールしてサインインすればプロファイルが流れ込んでくるって寸法よ。
まあ Node.js なんかの絡みがあるから、私の場合は引き続き WSL や devcontainer には頑張ってもらう必要があるのだけども。気にしない人なら、WSL のネットワーク設定だの Windows ファイルシステムとの相互アクセス遅延だのに悩む必要がなくなる分、楽ではある。

それを楽にできるのが Codespace なわけだが、わざわざ Codespace じゃなくて devcontainer+WSL2 とか Mac 買うてきてやればいいんでないのという話になる。
Codespace のメリットはブラウザから VSCode ライクなエディタでぶっ叩けること。この記事もブラウザから書いてます。
Docker を用いて執筆環境を構築するローカルの devcontainer と違って GitHub 所有のサーバにリモートで接続する形なので、ローカルの保存容量が少なくて済む。もしかしたら通信容量も…わからん。
↑そんなことはなかった、少なくとも新幹線では devcontainer のほうがいい。もちろん事前に devcontainer を構築しておくのが大前提だけんども。
最低限 Chrome が動けばいいのでスペックが安上がりでいい。ネカフェのどうしようもない PC から、図書館で借りたどうしようもない PC まで。一応 Chrome が動くくらいの誠意は見せてくれるはずなので。
↑甘い、かにみそくらい甘い。Chrome 全然動かないネカフェの PC も珍しくないんやなって京都で悟ったよ。Chrome も満足に動かない PC では、当然ブラウザ経由での Codespaces も当てにならん。devcontainer なんてもってのほかじゃな。大抵再起動のときに復元かかるし管理者権限ない。そもそも他人様の PC に Windows 機能を勝手に追加するバカ野郎を想定しておりませぬ。サポート外。

# デメリット。
拡張機能が対応していない場合あり。特にブラウザで叩く場合、プレビュー機能のある拡張機能は動かない可能性がある。
Zenn Editor のプレビュー機能はダメでした。
novel writer もダメでした。
textlint は大丈夫でした。
たぶん CLI 系は大丈夫。後述の dotfiles が公式で対応しているので。
↑ZennEditor と novel writer はいずれもプレビュー機能対応済でした。少なくとも今のところはな。しかし LaTeX をやろうとしてはならない、リアルタイムとはいかぬゆえな。
↑今はわからん。というかもうブラウザでやる意味ないとさえ思われる。みんなどうせ VSCode くらいインストールしてるでしょ。それなら devcontainer で十分という考え方もあるでな。Windows11Home でも WSL2 使えるし。Docker は WSL の Ubuntu にインストールしような。ディストリビューションにこだわる人間が Windows11Home なんて使ってるわけないやろ。ないよな。まあ Store にあれば WindowsTerminal 関連の設定も大抵なんとかなる感覚。