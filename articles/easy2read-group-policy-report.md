---
title: "グループポリシーの設定内容をわかりやすく出力する方法"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["windows","activedirectory","grouppolicy"]
published: true
---
# 概要

- GPO の設定内容を書き出す方法を調べた
- わかりやすいと嬉しい

# 経緯

納品ドキュメントにグループポリシーの設定内容を記載することになったが、内容が煩雑で転記したくなかった。

PowerShell か何かでうまいことできないか調べたところ、それなりにうまいことできそうな方法が見つかった。

## HTMLをExcelで開いて加工

PowerShell で下のコマンドを実行すると、PowerShell のカレントディレクトリに GPO と同名の HTML ファイルが作成される。

```powershell
Get-GPO -All | ForEach-Object -Process {Get-GPOReport -GUID $_.ID -ReportType html -Path .\$($_.Displayname).html}
```

それぞれ Excel で開き、加工すればいい。

![](https://storage.googleapis.com/zenn-user-upload/41ff00817579-20221101.png)

こんな感じ。パッと見にはキレイ。
あとは全選択して、罫線を消したりフォントを統一したりハイパーリンクを消したり、どうとでもなる。
体裁を整えて必要なところだけコピーして、PowerPoint に貼り付けたりもできる。

:::message
まだ PowerPoint で消耗してるの、とか言わない。
言い方きつい。
:::

ただ、GPO の数だけ HTML ファイルが作成されるのはツラい。

2 つか 3 つならいいが、10 個 20 個になると泣けてくる。

新しいものを少数追加した、というようなケースのときは便利に使える。

```powershell
Get-GPOReport -Name "<GPOの名前>" -ReportType HTML
```

その際は上記のコマンドがおすすめ。

### 2023 年 2 月 9 日追記
全 GPO の情報を含んだ HTML ファイルを吐き出す引数があったようなので、追記する。
```PowerShell
Get-GPOReport -All -ReportType Html -Path "C:\Work\All-GPOs.html"
```
もうちょい踏み込んで調べておけばよかった。

### 2023年2月15日 追記
上記の全 GPO 情報 HTML 吐き出し PowerShell だが、吐き出された HTML ファイルを Excel で読み込むと先頭にある GPO しか読み込んでこないことがわかった。
HTML の中身を見る限り、GPOReport の HTML ファイルをかなり雑に切り貼りしているだけに見える。

Python でパースしようと思えばできそうだが、イチから Python を書くよりは、GPO をひとつずつ吐き出して Excel で開くほうが早そうと判断した。
誰か GPOReport をパースしてくれるステキなスクリプトを書いてください。
頼みましたよ。

## XMLをExcelで開いて加工

しんどいのでもう少し調べてみた。

https://learn.microsoft.com/en-us/answers/questions/59083/exporting-gpo39s-to-excel.html

上記のページで、次の言及がある。

> You could use PowerShell to dump them to XML, then open via excel.
(PowerShell で XML に吐き出して、Excel で開くといいよ)
>

一緒に記載されているリンクは下記。

https://learn.microsoft.com/en-us/powershell/module/grouppolicy/get-gporeport?view=windowsserver2022-ps&viewFallbackFrom=win10-ps

HTML の際に使ったコマンドだが、オプションが違うようだ。

PowerShell で下のコマンドを実行すると、全 GPO の設定内容を含んだ XML ファイルが吐き出される。

```powershell
Get-GPOReport -All -ReportType XML
```

吐き出されたファイルを Excel で開くと、自動で表が作成される。

![](https://storage.googleapis.com/zenn-user-upload/e68728a5fea4-20221101.png)

こんなのが出てきて、

![](https://storage.googleapis.com/zenn-user-upload/e5d453962666-20221101.png)

適当に OK して、

![](https://storage.googleapis.com/zenn-user-upload/f0f1cc9fd6bd-20221101.png)

まあいいんじゃないですかね。

![](https://storage.googleapis.com/zenn-user-upload/efab8796a8bc-20221101.png)

こんな感じ。

(あくまで一部です)

作成・編集時間とか SID とか、ドキュメントには必ずしも必要とまでは言えない情報が多いので、それらを削除すればかなり見通しがよくなりそう。

ファイルがひとつにまとまっているのが何より嬉しい。

# まとめ

基本的に PowerShell を経由して Excel に帰着するのがよろしいのではないか。

ADRecon なんかもいいかなと思ったが、やっていることは大枠同じのようだ。

https://github.com/adrecon/ADRecon

業務上の要請で諸々調べているうちに、キラキラ SIer はどういうドキュメントに起こしているのか興味が出てきた。