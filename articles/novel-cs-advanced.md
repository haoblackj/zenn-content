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
それを楽にできるのが Codespace なわけだが、わざわざ Codespace じゃなくて devcontainer+WSL2 とか Mac 買うてきてやればいいんでないのという話になる。
Codespace のメリットはブラウザから VSCode ライクなエディタでぶっ叩けること。この記事もブラウザから書いてます。
Docker を用いて執筆環境を構築するローカルの devcontainer と違って GitHub 所有のサーバにリモートで接続する形なので、ローカルの保存容量が少なくて済む。もしかしたら通信容量も…わからん。
最低限 Chrome が動けばいいのでスペックが安上がりでいい。ネカフェのどうしようもない PC から、図書館で借りたどうしようもない PC まで。一応 Chrome が動くくらいの誠意は見せてくれるはずなので。

# デメリット。
拡張機能が対応していない場合あり。特にブラウザで叩く場合、プレビュー機能のある拡張機能は動かない可能性がある。
Zenn Editor はダメでした。