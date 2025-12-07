---
title: "自宅サーバ2025"
emoji: "🏠"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["自宅サーバ","Proxmox"]
published: false
---

<!-- textlint-disable -->
この記事は、[『みすてむず いず みすきーしすてむず その2 Advent Calendar 2025』](https://adventar.org/calendars/11627)の 7 日目の記事です。
<!-- textlint-enable -->

# 概要
2025 年現在の自宅サーバ構成と運用方針についての紹介記事。
1 時間くらいでざっと書いたので、細かいところはご容赦を。

# 構成
## ハードウェア
ThinkCentre M73 Tiny (3 台)
- CPU: Intel Core i3-4170T
- RAM: 16GB (増設)
- ストレージ: 500GB SSD
- ネットワーク: 有線 LAN

Dell 製中古ゲーミング PC (1 台)
- CPU: Intel Core i7-7700
- RAM: 32GB
- ストレージ: 1TB SSD + 2TB HDD
- ネットワーク: 有線 LAN

Synology DS918+ (1台)
- ストレージ: 4 x 4TB HDD (Synology Hybrid RAID)

## ソフトウェア
- OS: Proxmox VE 8

## サービス
- Active Directory Domain Service (Windows Server 2022)
- Dashy (ダッシュボード)
- Prometheus + Grafana (監視)
- Portainer (コンテナ管理)
- LibreChat (LLMチャットサービス)
- OpenWebUI (LLMチャットサービス)
- Cloudflare Tunnel + Access (リバースプロキシ)
- SoftEther VPN (VPNサービス)

# 経緯
新卒の頃は手元のPCのHyper-Vで満足できていたのが、Synology DS918+を購入したのがケチのつき始め。
やっぱりVMはNASにバックアップしてナンボだよな、といろいろ仮想環境について調べ始め、Proxmox VEにたどり着く。
ThinkCentre M73 Tinyがタダみたいな値段で売っていたので 3 台購入し、Proxmox VEクラスタを構築。
その後、やっぱりタダみたいな値段で売っていたDell製中古ゲーミングPCを購入し、Proxmox VEクラスタに追加。こいつには主にDockerコンテナを取りまとめたUbuntu VMを載せている。
DS918+はストレージ専用機として、Proxmox VEクラスタやメインマシンのバックアップ先として活躍中。Cloudflaredの予備コンテナも動かしている。

# 運用方針
できるだけ手をかけず安定稼働。これしかありますまい。
DockerコンテナはPortainerで管理し、VMはProxmox VEのWebUIで管理。なるべくコマンドラインには触らない。本職じゃないので。
VMのバックアップはProxmox Backup Serverを使い、DellPCのHDD経由でDS918+に保存。毎日フルバックアップだ。重複排除はProxmox Backup Serverにお任せ。
監視はPrometheus + Grafanaで行い、異常があればメールで通知されるようにしている。
外部からのアクセスはCloudflare Tunnel + Accessのみに限定し、M365認証を併用してセキュリティを確保。
SoftEther VPNも用意しているが、普段は使わず緊急時用。持ち出しPCに入れていなくて焦るところまでセット。

# こだわりポイント
とにかくバックアップだ！
Proxmox VE の VM は Proxmox Backup Server で主要 VM を毎日バックアップ。
Synology DS918+の Active Backup for Business で、メインマシンを毎日バックアップ。
DS918+自体も Hyper Backup で外部ストレージに日 1 回バックアップ。

LibreChat には過去からのやり取り履歴が連綿と積み重なっているので、データベースバックアップには特に気を使っている。
Ubuntu VM 上で定期的にデータベースダンプを取得し、VM ごと Proxmox Backup Server でバックアップ。DS918+にも VM 内から別途バックアップを保存している。
OpenWebUI にはバックアップ機能がなく、データベースをコンテナとして分離もできないので、コンテナの永続ボリュームを VM ごと Proxmox Backup Server でバックアップしている。

メインマシンや持ち出し PC は Active Directory に参加させ、グループポリシーで各種設定を一元管理。
いつどうなってもいいようにしている。

# 今後の展望
自宅サーバの運用は安定しているが、ハードウェアの老朽化が気になるところ。
メインマシンのリプレースが決定したので、旧メインマシンを自宅サーバに組み込む予定。
Synology DS918+の後継機も検討中。もう8年も動かしているので、いろいろガタが来てそう。

財布が夢くらい大きくなるといいのですけれど。

# まとめ
みんなも使おうProxmox
みんなも取ろうバックアップ