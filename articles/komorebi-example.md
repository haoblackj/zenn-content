---
title: "komorebiの設定例"
emoji: "🪟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["komorebi", "windows", "windowmanager"]
published: false
published_at: 2024-12-10 08:55
---

<!-- textlint-disable -->
この記事は、[『みすてむず いず みすきーしすてむず その2 Advent Calendar 2024』](https://adventar.org/calendars/10269)の 10 日目の記事です。
<!-- textlint-enable -->

# はじめに
`komorebi` というタイル型ウィンドウマネージャがある。
奇特にも Windows に特化した WM だ。
悲しいことに、`komorebi` という名前がついているのに日本語の情報が少ない。
公式でも、機能によっては動画解説だけだったりもする。

なので、この記事で `komorebi` の設定例を紹介することにした。

# インストール
`winget` もしくは `scoop` でインストールできる。
後述する `whkd` を使いたい場合は、ここで同時にインストールしておくとよい。
古式ゆかしい `AutoHotkey` でも動作するらしいが、私は既存で `AutoHotkey` を使っているので、採用しなかった。
詳しくは[公式サイト](https://lgug2z.github.io/komorebi/installation.html)をチェックだ。

Windows でたまに取り沙汰される『パスの最大長の設定』についても記載があるので、任意に設定するとよろしい。私はしてない。
アニメーション無効化についての推奨設定についても記載があるが、これも任意だ。

# 起動
`komorebic start --whkd --bar` で起動できる。
私は OS 起動時に手動で PowerShell から起動している。
セキュリティソフトなどのスプラッシュスクリーンも制御されると目障りなためだ。

自動起動させるためのコマンドもあるにはあるが、どうもうまく動かないことが多いらしい。
上記のコマンドを `.ps1` ファイルに起こして、スタートアップフォルダに放り込むのが確実だ。

# 設定例
おまちかねの設定例だ。
`komorebi` は `komorebi.json` ファイルで設定する。

```json:komorebi.json
{
  "$schema": "https://raw.githubusercontent.com/LGUG2Z/komorebi/v0.1.30/schema.json",
  "app_specific_configuration_path": "$Env:KOMOREBI_CONFIG_HOME/applications.json",
  "window_hiding_behaviour": "Cloak",
  "cross_monitor_move_behaviour": "Insert",
  "animation": {
    "enabled": false
  },
  "default_workspace_padding": 0,
  "default_container_padding": 0,
  "border": true,
  "border_width": 4,
  "border_offset": -1,
  "theme": {
    "palette": "Base16",
    "name": "HorizonDark",
    "unfocused_border": "Base03",
    "bar_accent": "Base0D"
  },
  "stackbar": {
    "height": 40,
    "mode": "OnStack",
    "tabs": {
      "width": 300
    }
  },
  "monitors": [
    {
      "workspaces": [
        {
          "name": "Primary",
          "layout": "VerticalStack"
        }
      ]
    },
    {
      "workspaces": [
        {
          "name": "Secondary",
          "layout": "UltrawideVerticalStack"
        }
      ]
    },
    {
      "workspaces": [
        {
          "name": "Tertiary",
          "layout": "Rows"
        }
      ]
    },
    {
      "workspaces": [
        {
          "name": "Quaternary",
          "layout": "VerticalStack"
        }
      ]
    }
  ],
  "display_index_preferences": {
    "0": "<モニターのid>",
    "1": "<モニターのid>",
    "2": "<モニターのid>",
    "3": "<モニターのid>"
  }
}
```
## 用語の解説
- コンテナ: ひとつ以上のウィンドウを束ねたセット。コンテナ内のウィンドウはスタックされる。コンテナごとのレイアウトを変更することで、ウィンドウの配置を変更している。
- ワークスペース: 複数のコンテナをまとめたセット。ワークスペースはコンテナのサイズや位置を管理する。ワークスペースごとに異なるレイアウトを設定できる。
- モニター:複数のワークスペースをまとめたセット。用途ごとに切り替えることで仮想デスクトップのように使うことができる。OS 標準の仮想デスクトップとは相性が悪い。物理的なモニター(ディスプレイ)と紐づけることも可能。
- スタックバー: コンテナ内でスタックされたウィンドウの情報を表示するタブバー。デフォルト設定では `Alt-Tab` の操作と干渉してしまうので、使っていない。
- `whkd`: キーボードショートカットを設定するためのツール。`komorebi` でインストールされたコマンドを `whkd` で設定したキーボードショートカットで呼び出すというイメージ。`komorebi` と同時にインストールするとよい。

## 設定の解説
- `app_specific_configuration_path`: アプリケーションごとの設定ファイルのパス。`komorebi` で配置を制御したくない・不具合を起こすアプリケーションが記載されている。`komorebi` ユーザが情報を持ち寄って作成したファイルがあるので、大抵はそれを使ったほうがいい。`PowerShell` もしくは `cmd` から `komorebic fetch-asc` で取得できる。
- `window_hiding_behaviour`: ウィンドウを隠す際の挙動。`Cloak` はウィンドウを隠す。`Hide` はウィンドウを非表示にする。`minimize` は最小化する。ワークスペース遷移時の挙動に差が出るみたいだが、ワークスペースを使ってないのでよくわからん。
- `cross_monitor_move_behaviour`: モニター間でウィンドウを移動する際の挙動。`Insert` はウィンドウを挿入する。`Swap` はウィンドウを入れ替える。`NoOp` は何もしない。
- `animation`: アニメーションの有効・無効。`true` で有効、`false` で無効。……らしいが、正直言って違いがわからなかった。
- `default_workspace_padding`: ワークスペースのパディング。`0` でパディングなし。
- `default_container_padding`: コンテナのパディング。`0` でパディングなし。
- `border`: ウィンドウの枠線の有無。`true` で枠線あり、`false` で枠線なし。あったほうが見やすいが、極まったユーザにとっては邪魔らしい……。
- `border_width`: ウィンドウの枠線の幅。`4` で 4 ピクセルの幅。
- `border_offset`: ウィンドウの枠線のオフセット。この辺はよくわかってないが、とりあえず `-1` に。
- `theme`: テーマの設定
  - `palette` :カラーパレット。`Cappuccin` パレットか `Base16` パレットを選択できる
  - `name` : テーマ名。`Base16` パレットのテーマは大抵使えるらしい
  - `unfocused_border` : 非アクティブウィンドウの枠線色
  - `bar_accent` : Stackbar のアクセント色
- `stackbar`: Stackbar の設定。前述のとおり私は使っていないので、デフォルトの設定のまま。
  - `height` : Stackbar の高さ
  - `mode` : Stackbar の表示モード、`OnStack` か `Never` か `Always` が選択できる
  - `tabs` : Stackbar のタブの設定。`width` でタブの幅を指定できる
  - `label` : タブのラベルに `Process` もしくは `Title` を指定できる
- `monitors`: モニターの設定。
  - `workspaces` : ワークスペースを設定する
    - `name` : ワークスペースの名前を指定する
    - `layout` : レイアウトを指定する。`komorebi` にはいくつかのデフォルトレイアウトがあるので、それを使うとよい。詳しくは[ここ](https://lgug2z.github.io/komorebi/example-configurations.html#layouts)を参照。
- `display_index_preferences`: モニターのインデックスとディスプレイの ID の対応表。ディスプレイの ID は `komorebic monitor-information` で取得できる^[とっても紛らわしいが、慣れがすべてを解決する]。

`komorebi.json` は設定変更をかければ、基本的には即時反映される。
時折反映されないことがあるので、下記コマンドで再読み込みするのが確実。
```powershell
komorebic stop --whkd --bar; komorebic start --whkd --bar
```
後述の `bar` 関連の設定などは、ほぼ再読み込みしないと反映されない。
`whkd` に至っては、OS を再起動しないと反映されないこともしばしばだ。
Windows では再起動はすべてを解決する。

# 設定例(bar)
`komorebi` にはステータスバー機能がある。
いまフォーカスしているウィンドウの情報や、再生中のメディア情報、日時やバッテリー残量などを表示できる。
設定は `komorebi.bar.json` に記述する。

```json:komorebi.bar.json
{
  "$schema": "https://raw.githubusercontent.com/LGUG2Z/komorebi/v0.1.30/schema.bar.json",
  "monitor": {
    "index": 0,
    "work_area_offset": {
      "left": 0,
      "top": 50,
      "right": 0,
      "bottom": 50
    }
  },
  "font_family": "PlemolJP Console HS",
  "theme": {
    "palette": "Base16",
    "name": "HorizonDark",
    "accent": "Base0D"
  },
  "left_widgets": [
    {
      "Komorebi": {
        "workspaces": {
          "enable": true,
          "hide_empty_workspaces": true
        },
        "layout": {
          "enable": true
        },
        "focused_window": {
          "enable": true,
          "show_icon": true
        }
      }
    }
  ],
  "right_widgets": [
    {
      "Media": {
        "enable": true
      }
    },
    {
      "Date": {
        "enable": true,
        "format": "YearMonthDate"
      }
    },
    {
      "Time": {
        "enable": true,
        "format": "TwentyFourHour"
      }
    },
    {
      "Battery": {
        "enable": true
      }
    }
  ]
}
```

## 設定の解説
- `monitor`: ステータスバーの表示位置を指定する。`index` でモニターのインデックスを指定する。`work_area_offset` でステータスバーとコンテナの位置関係を制御する。
- `font_family`: フォントファミリを指定する。`PlemolJP Console HS` は私のお気に入り。
- `theme`: テーマの設定。`komorebi.json` と同じ要領。
- `left_widgets`: ステータスバーの左側に表示するウィジェットの設定。
  - `Komorebi` : `komorebi` に関する情報を表示するウィジェット。
    - `workspaces` : ワークスペースの情報を表示する。
    - `layout` : レイアウトの情報を表示する。
    - `focused_window` : フォーカスしているウィンドウの情報やアイコンを表示する。2 バイト文字対応フォントを指定しないと豆腐で表示されるので注意。
- `right_widgets`: ステータスバーの右側に表示するウィジェットの設定。
  - `Media` : 再生中のメディア情報を表示する。
  - `Date` : 日付を表示する。`format` で表示形式を指定できる。
  - `Time` : 時刻を表示する。`format` で表示形式を指定できる。
  - `Battery` : バッテリー残量を表示する。

他にもいくつかのウィジェットがあるので、[公式ドキュメント](https://lgug2z.github.io/komorebi/example-configurations.html#komorebibarjson)を参照するとよい。

# 設定例(whkd)
`komorebi` をキーボードから制御するには、`whkd` が必要だ。
`whkdrc` に下記を保存すれば、`komorebi` をキーボードから制御できる。

```whkdrc:whkdrc
.shell powershell

# Reload whkd configuration
# alt + o                 : taskkill /f /im whkd.exe && start /b whkd # if shell is cmd
alt + o                 : taskkill /f /im whkd.exe; Start-Process whkd -WindowStyle hidden # if shell is pwsh / powershell
alt + shift + o         : komorebic reload-configuration

# App shortcuts - these require shell to be pwsh / powershell
# The apps will be focused if open, or launched if not open
# alt + f                 : if ($wshell.AppActivate('Firefox') -eq $False) { start firefox }
# alt + b                 : if ($wshell.AppActivate('Chrome') -eq $False) { start chrome }

alt + q                 : komorebic close
alt + m                 : komorebic minimize

# Focus windows
alt + h                 : komorebic focus left
alt + j                 : komorebic focus down
alt + k                 : komorebic focus up
alt + l                 : komorebic focus right
alt + shift + oem_4     : komorebic cycle-focus previous # oem_4 is [
alt + shift + oem_6     : komorebic cycle-focus next # oem_6 is ]

# Move windows
alt + shift + h         : komorebic move left
alt + shift + j         : komorebic move down
alt + shift + k         : komorebic move up
alt + shift + l         : komorebic move right
alt + shift + return    : komorebic promote
alt + right     : komorebic cycle-monitor previous # oem_3 is @
alt + left	 : komorebic cycle-monitor next # oem_1 is ;

# Stack windows
#alt + left              : komorebic stack left
#alt + down              : komorebic stack down
#alt + up                : komorebic stack up
#alt + right             : komorebic stack right
#alt + oem_1             : komorebic unstack # oem_1 is ;
#alt + oem_4             : komorebic cycle-stack previous # oem_4 is [
#alt + oem_6             : komorebic cycle-stack next # oem_6 is ]

# Resize
alt + oem_plus          : komorebic resize-axis horizontal increase
alt + oem_minus         : komorebic resize-axis horizontal decrease
alt + shift + oem_plus  : komorebic resize-axis vertical increase
alt + shift + oem_minus : komorebic resize-axis vertical decrease

# Manipulate windows
alt + t                 : komorebic toggle-float
alt + shift + f         : komorebic toggle-monocle

# Window manager options
alt + shift + r         : komorebic retile
alt + p                 : komorebic toggle-pause

# Layouts
alt + x                 : komorebic flip-layout horizontal
alt + y                 : komorebic flip-layout vertical
alt + oem_4			 : komorebic cycle-layout previous # oem_4 is [
alt + oem_6			 : komorebic cycle-layout next # oem_6 is ]

# Workspaces
alt + 1                 : komorebic focus-workspace 0
alt + 2                 : komorebic focus-workspace 1
alt + 3                 : komorebic focus-workspace 2
alt + 4                 : komorebic focus-workspace 3
alt + 5                 : komorebic focus-workspace 4
alt + 6                 : komorebic focus-workspace 5
alt + 7                 : komorebic focus-workspace 6
alt + 8                 : komorebic focus-workspace 7

# Move windows across workspaces
alt + shift + 1         : komorebic move-to-workspace 0
alt + shift + 2         : komorebic move-to-workspace 1
alt + shift + 3         : komorebic move-to-workspace 2
alt + shift + 4         : komorebic move-to-workspace 3
alt + shift + 5         : komorebic move-to-workspace 4
alt + shift + 6         : komorebic move-to-workspace 5
alt + shift + 7         : komorebic move-to-workspace 6
alt + shift + 8         : komorebic move-to-workspace 7
```

基本的には見たままで、`komorebi` のコマンドを `whkd` で呼び出しているだけだ。
コマンドリファレンスは[公式ドキュメント](https://lgug2z.github.io/komorebi/index.html)に整備されているので、そちらを参照するとよい。

# 環境変数の設定
`komorebi` の設定ファイルは、デフォルトでは `$Env:USERPROFILE` に配置される。
とりあえず使い始めたい場合は、何も考えずに `komorebic quickstart` を実行するといい。
ドキュメントに紹介されている初期設定が読み込まれて、`komorebi` が起動する。

もし設定ファイルの配置先を変更したい場合は、環境変数として `$Env:KOMOREBI_CONFIG_HOME` を設定する必要がある。

なお、`whkd` の設定ファイルも同じ要領で `$Env:WHKD_CONFIG_HOME` を設定する。

```powershell:Microsoft.PowerShell_profile.ps1
$Env:KOMOREBI_CONFIG_HOME = "C:\Users\$($env:USERNAME)\.config\komorebi"
$Env:WHKD_CONFIG_HOME = "C:\Users\$($env:USERNAME)\.config\whkd"
```

上記を `Microsoft.PowerShell_profile.ps1` に追記して、ドキュメントフォルダの下記に保存する。
- PowerShell 7: `C:\Users\<ユーザ名>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1`
- PowerShell 5: `C:\Users\<ユーザ名>\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`

# おわりに
`komorebi` は Windows でタイル型ウィンドウマネージャを使うという奇特な試みの地平線だ。
<!-- textlint-disable -->`whkd` の設定次第で、Windows でありながらホームポジションから離れずに GUI を操作できるようになる^[Chromiumブラウザは `Vimmium` を使えばいい。他のGUIアプリは……知らん]。
<!-- textlint-enable -->
デュアルモニターに弱かったり、ゴーストウィンドウ問題が残ったままだったりと、`komorebi` にはまだまだ改善の余地がある。
スタートアップ登録がうまくいかない問題は、結構な数のユーザが改善を待ちわびているようだ。

それでも、キーボード操作ひとつでウィンドウサイズや位置を調整できるというのは、Windows デフォルト状態では望むべくもない快適さだ。

みんなも使おう。