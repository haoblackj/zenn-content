---
title: "Proxmox VE 7 クラスタ構築した(Lenovo M73 Tiny × 2台)"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["proxmox","windowsserver"]
published: false
---

# 要点
- Proxmox VE クラスタ環境を構築した
- Windows Server 2022 のテンプレートもあわせて作成した
- ノードの IP アドレス変更時の作法は大事

# 経緯
先日、安く Lenovo M73 Tiny を 2 台仕入れた。
自宅で稼働する物理 AD サーバがお亡くなりになったためだ。
AD 以外は DNS と DHCP くらいしか動かしていないので、安っぽいお弁当箱型 PC でも問題ないと思ったのだ。

しかし、M73 Tiny には落とし穴があった。
Windows Server 2022 をそのままインストールすると、NIC を認識しないのである。
Lenovo からドライバを調達してインストールしようとしたところ、 **「LAN ケーブルが刺さっていないから中断するね」** 旨のありがたいお言葉をいただいた。
ベンダーコードのご厚情と思われる。

書き換えも面倒だったので、前々から気になっていた Proxmox をインストールしてみることにした。
すると思ったよりも簡単にできたので、手順書代わりに共有しようと思い立ったわけである。

# 手順
## 前提条件
以下のものが必要になる。
- Proxmox 用端末(Lenovo M73 Tiny)
- 作業用 Windows 端末(Rufus と Tera Term が動けば OK)
- USB メモリ(32GB もあれば十分)
- 仮想マシンの OS インストーラー(ISO ファイルのままで問題ありません)

また、作業用 Windows 端末には次のソフトウェアが必要になる。
- Rufus
https://rufus.ie/ja/
- Tera Term
https://github.com/TeraTermProject/osdn-download/releases

## BIOS 設定
BIOS 設定は以下の通り。
- UEFI モードで起動する
- セキュアブートを無効にする
- ネットワークブートを有効にする
- ブートオーダーを USB に変更する

## Proxmox のインストール
USB メモリに Rufus で Proxmox の ISO ファイルを書き込む。
書き込みが完了したら、USB メモリを M73 Tiny に挿入して起動する。
インストーラーが起動したら、以下の通りに進める。
- Install Proxmox VE
- Accept license agreement
- Select Target Device
- Select Country
- Set Root Password
- Network
  - Hostname: proxmox1
  - IP Address:環境による
  - Gateway:環境による
  - DNS:環境による
- Confirm Installation
- Installation Progress
- Reboot

## ネットワーク設定
Proxmox のインストールが完了したら、ブラウザで以下の URL にアクセスする。
`https://[上記で設定したIPアドレス]:8006`