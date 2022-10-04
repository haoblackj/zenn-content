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
小説の執筆環境は肥大化する。
VSCode にはそれを促す傾向がある。

自動保存やウィンドウレイアウトの状態に始まり、キャレットの見た目やエディタのテーマなど、VSCode の設定はいくらでも作り込める。
改行コード統一や Git 周辺の操作など、便利な拡張機能は数え切れない。

問題は、その設定の変更を端末ごとに実施・規正しなければならないという点だ。

拡張機能の有無なら、その機能を再インストールすれば事足りる。
しかし細かい設定ではそうもいかない。
設定画面を開いてクリック、あるいは `setting.json` をどうにかして上書きするしかない。
快適さとは程遠い。

とはいえ、複数の端末を使って執筆したいというのは極めて一般的な要望だ。
できれば、慣れた同じ環境で執筆したい。

そこで登場するのが、GitHub Codespaces である。