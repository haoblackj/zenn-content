---
title: "グループポリシーの設定内容をわかりやすく出力する方法"
emoji: "🗂"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
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

![Untitled](%E3%82%AF%E3%82%99%E3%83%AB%E3%83%BC%E3%83%95%E3%82%9A%E3%83%9B%E3%82%9A%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%AE%E8%A8%AD%E5%AE%9A%E5%86%85%E5%AE%B9%E3%82%92%E3%82%8F%E3%81%8B%E3%82%8A%E3%82%84%E3%81%99%E3%81%8F%E5%87%BA%E5%8A%9B%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%202f74d79a4170468a9502614b4b25f812/Untitled.png)

こんな感じ。パッと見にはキレイ。

ただ、GPO の数だけ HTML ファイルが作成されるのはツラい。

2 つか 3 つならいいが、10 個 20 個になると泣けてくる。

新しいものを少数追加した、というようなケースのときは便利に使える。

```powershell
Get-GPOReport -Name "<GPOの名前>" -ReportType HTML
```

その際は上記のコマンドがおすすめ。

## XMLをExcelで開いて加工

しんどいのでもう少し調べてみた。

[Exporting GPO's to Excel - Microsoft Q&A](https://learn.microsoft.com/en-us/answers/questions/59083/exporting-gpo39s-to-excel.html)

上記のページで、次の言及がある。

> You could use PowerShell to dump them to XML, then open via excel.
(PowerShell で XML に吐き出して、Excel で開くといいよ)
>

一緒に記載されているリンクは下記。

[Get-GPOReport (GroupPolicy)](https://learn.microsoft.com/en-us/powershell/module/grouppolicy/get-gporeport?view=windowsserver2022-ps&viewFallbackFrom=win10-ps)

HTML の際に使ったコマンドだが、オプションが違うようだ。

PowerShell で下のコマンドを実行すると、全 GPO の設定内容を含んだ XML ファイルが吐き出される。

```powershell
Get-GPOReport -All -ReportType XML
```

吐き出されたファイルを Excel で開くと、自動で表が作成される。

![Untitled](%E3%82%AF%E3%82%99%E3%83%AB%E3%83%BC%E3%83%95%E3%82%9A%E3%83%9B%E3%82%9A%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%AE%E8%A8%AD%E5%AE%9A%E5%86%85%E5%AE%B9%E3%82%92%E3%82%8F%E3%81%8B%E3%82%8A%E3%82%84%E3%81%99%E3%81%8F%E5%87%BA%E5%8A%9B%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%202f74d79a4170468a9502614b4b25f812/Untitled%201.png)

こんなのが出てきて、

![Untitled](%E3%82%AF%E3%82%99%E3%83%AB%E3%83%BC%E3%83%95%E3%82%9A%E3%83%9B%E3%82%9A%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%AE%E8%A8%AD%E5%AE%9A%E5%86%85%E5%AE%B9%E3%82%92%E3%82%8F%E3%81%8B%E3%82%8A%E3%82%84%E3%81%99%E3%81%8F%E5%87%BA%E5%8A%9B%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%202f74d79a4170468a9502614b4b25f812/Untitled%202.png)

適当に OK して、

![Untitled](%E3%82%AF%E3%82%99%E3%83%AB%E3%83%BC%E3%83%95%E3%82%9A%E3%83%9B%E3%82%9A%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%AE%E8%A8%AD%E5%AE%9A%E5%86%85%E5%AE%B9%E3%82%92%E3%82%8F%E3%81%8B%E3%82%8A%E3%82%84%E3%81%99%E3%81%8F%E5%87%BA%E5%8A%9B%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%202f74d79a4170468a9502614b4b25f812/Untitled%203.png)

まあいいんじゃないですかね。

![Untitled](%E3%82%AF%E3%82%99%E3%83%AB%E3%83%BC%E3%83%95%E3%82%9A%E3%83%9B%E3%82%9A%E3%83%AA%E3%82%B7%E3%83%BC%E3%81%AE%E8%A8%AD%E5%AE%9A%E5%86%85%E5%AE%B9%E3%82%92%E3%82%8F%E3%81%8B%E3%82%8A%E3%82%84%E3%81%99%E3%81%8F%E5%87%BA%E5%8A%9B%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95%202f74d79a4170468a9502614b4b25f812/Untitled%204.png)

こんな感じ。

(あくまで一部です)

作成・編集時間とか SID とか、ドキュメントには必ずしも必要とまでは言えない情報が多いので、それらを削除すればかなり見通しがよくなりそう。

ファイルがひとつにまとまっているのが何より嬉しい。

# まとめ

基本的に PowerShell を経由して Excel に帰着するのがよろしいのではないか。

ADRecon なんかもいいかなと思ったが、やっていることは大枠同じのようだ。

[https://github.com/adrecon/ADRecon](https://github.com/adrecon/ADRecon)

業務上の要請で諸々調べているうちに、キラキラ SIer はどういうドキュメントに起こしているのか興味が出てきた。