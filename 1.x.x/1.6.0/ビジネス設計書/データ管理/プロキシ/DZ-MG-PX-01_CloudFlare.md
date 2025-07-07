# CloudFlare

---

### 当ページメタ情報

ページID：DZ-MG-PX-01

ページ名：CloudFlare

ファイル名：DZ-MG-PX-01_CloudFlare.md

ページ目的：CloudFlareの構成

ページ構成：ビジネス設計書＞データ管理＞プロキシ＞CloudFlare

最終更新：2025/7/2

変更点：ー

---

# TODO

- 

以下、残項目です。

| No. | 項目 | 内容 | 備考 |
| --- | --- | --- | --- |
| 3 | AIOWPS | • 「設定＞高度な設定＞リバースプロキシを許可」にチェック
• IPアドレスの取得方式が正しくなるか確認 |  |
| 4 | ページルール最終調整・動作テスト | 問題なければ運用開始 |  |

## 実装手順：WAF設定

---

### ▼ 5. 完了後、必ず動作テスト

- 管理者自身が日本IPからWordPress管理画面・ログイン画面に正常アクセスできるか確認
- 海外VPN等からアクセスを試す・遮断されることを確認
- サイト全体へのアクセスや検索エンジンのクロールも念のためチェック

---

## ◎ まとめ：シンプルなおすすめ構成例

1. **/wp-login.php, /wp-admin へのアクセス制限（日本IPのみに絞る）**
2. **国=Japan以外は全サイトブロック or Challenge**
3. **既知のBotや危険なUAはブロック**
4. **一時的なIPアドレスブロック・許可もToolsで対応可**

# CloudFlare プロキシ設定（基本情報）

## 割り当てネームサーバー情報

- [**brady.ns.cloudflare.com**](http://brady.ns.cloudflare.com/)
- [**ullis.ns.cloudflare.com**](http://ullis.ns.cloudflare.com/)

## サイト適用ドメイン

- [waterme.jp](http://waterme.jp/)

## 初期設定

### CloudFlare移行時の主要DNSレコード（2025/07/02時点）

- Aレコード
    - サイト本体とサブドメインへのアクセス制御・転送
    
    | 名前 | IPv4 アドレス | 役割 |
    | --- | --- | --- |
    | * | 162.43.116.72 | サブドメイン全体への転送　例：https://abc.waterme.jp/・https://blog.waterme.jp/ |
    | waterme.jp | 162.43.116.72 | メインサイト本体へのアクセス 例：”https://waterme.jp/anypage” |
    | www | 162.43.116.72 | “www.waterme.jp”アクセスを転送 |
- MXレコード
    - サイト用メール（@waterme.jp）受信
    
    | 名前 | メール サーバー | 役割 |
    | --- | --- | --- |
    | waterme.jp | waterme.jp | サイト用メール受信サーバ |
- TXTレコード
    - メール正当性の保証（DKIM, SPF, DMARC）
    - Google等のサイト所有権認証
    
    | 名前 | コンテンツ | 役割 |
    | --- | --- | --- |
    | default._domainkey | "v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzU6754GEeKzzYA5OGSJdbwkX0yPoVyrf1Bnhnhgv2AeoM3OiJKlNXAbNzH/+TQinW1/TSpybGzChdo7vtlAHnbvI3jk7Yzd1oD0kN2H0eYKQeiY5JUbeQnFJu6rrywbyV39V/bcOn1dDuHHwCZASloBjSWNxq6UWzh+lPpltl/0G2ZnTLq468Ddu2abQFyzOE" "Le7GJqBGd8/tfEIi5YEDsCx6WsZoT9wagJWQRqhqbNYNnlwzwa0NqRImBPGxxBkxuhpRLaYmf/ZTT3jLuo6MMNMSBIo1LYF6lhMQjvCURZblfuVhOqQcQTvWp524Y1N0peSCgdUCUxVu3e96cfSNQIDAQAB” | メール送信時の**DKIM電子署名認証** |
    | _dmarc | "v=DMARC1; p=none; rua=mailto:waterme.jp@gmail.com;" | メールなりすまし防止（**DMARC認証**） |
    | [waterme.jp](http://waterme.jp/) | "google-site-verification=LzXqE0IGiAn3HEVxh257etpYJalDJoIZ308NpkDNFA0” | Google Search Console用**サイト認証** |
    | [waterme.jp](http://waterme.jp/) | "v=spf1 +a:sv13071.xserver.jp +a:waterme.jp +mx include:spf.sender.xserver.jp include:_spf.google.com ~all” | メール送信元認証（**SPFレコード**） |
- 旧NSレコード
    - CloudFlare移行前の履歴記録（万一の復元や参考用）
    
    | 名前 | メール サーバー | 役割 |
    | --- | --- | --- |
    | waterme.jp | ns1.xserver.jp | 旧NSレコードの一時的記録 |
    | waterme.jp | ns2.xserver.jp | 旧NSレコードの一時的記録 |
    | waterme.jp | ns3.xserver.jp | 旧NSレコードの一時的記録 |
    | waterme.jp | ns4.xserver.jp | 旧NSレコードの一時的記録 |
    | waterme.jp | ns5.xserver.jp | 旧NSレコードの一時的記録 |
- CNAMEレコード
    - 別のドメイン名を参照（転送）するDNSレコード。IPアドレスではなく、“他のホスト名（ドメイン名）”を指定します。
    - 例：blog.waterme.jpアクセス → example.netへアクセス転送
    - **（現時点で利用なし／必要な場合追記）**

### 設定ステータス

- 設定作業：2025-07-02時点でアクティベーション済み

---

# 運用メモ

- CloudFlareネームサーバーは、**ドメイン管理会社側で設定し、CloudFlare管理ダッシュボードでのみ変更可能となる**
- 今後のDNS編集・追加は、**CloudFlare側で一元管理**
- トラブル時は、**割り当てネームサーバー情報を管理者へ提示しやすくするため必ずここを確認**

---

# WAF設定：拒否

### ルール一覧（2025/07/03時点）

| **ルール名** | **条件（式）** | **アクション** | **順序（優先度）** | **補足** |
| --- | --- | --- | --- | --- |
| 管理画面・ログインページの保護 | ((http.request.uri.path eq "/wp-login.php") or (http.request.uri.path contains "/wp-admin" )) and (ip.src.country ne "JP") | ブロック | 1（最優先） | 日本以外からの管理画面・ログインページ遮断 |
| 海外からの全アクセス制限 | (ip.src.country ne "JP") | ブロック | 2 | 日本以外からの全アクセス遮断 |
| 悪質Bot・不審UserAgentブロック | http.user_agent contains "AhrefsBot"or http.user_agent contains "SemrushBot"or http.user_agent contains "DotBot"or http.user_agent contains "MJ12bot"or http.user_agent contains "Baiduspider"or http.user_agent contains "YandexBot"or http.user_agent contains "PetalBot"or http.user_agent contains "Sogou"or http.user_agent contains "spbot"or http.user_agent contains "BLEXBot"or http.user_agent contains "Exabot"or http.user_agent contains "Cliqzbot"or http.user_agent contains "MegaIndex"or http.user_agent contains "Seekport"or http.user_agent contains "SeznamBot"or http.user_agent contains "python-requests"or http.user_agent contains "python"or http.user_agent contains "Go-http-client"or http.user_agent contains "curl"or http.user_agent contains "wget"or http.user_agent contains "libwww-perl"or http.user_agent contains "Java/"or http.user_agent contains "urllib"or http.user_agent contains "scrapy"or http.user_agent contains "node-fetch" | ブロック | 3（最後） | 代表的なBot/スクレイピングツール等の一括遮断 |

---

### 設定方法

1. **CloudFlareダッシュボード** →「Security」→「WAF」→「カスタムルール」タブ
2. 「ルールを作成（Add rule）」をクリック
3. GUIで大まかに条件を設定→**[式を編集]で上記式をそのまま貼り付け**
4. アクションは「ブロック（Block）」を選択
5. ルール名・順序を明確に設定（優先度順）
6. 追加・修正・削除は同画面から随時可能

### 補足

- *ルールは上から順に適用。**管理画面保護→全体海外遮断→Bot遮断の順推奨
- 新しいBotや不審UserAgentが検出された場合は、**随時「or」で追加可能**
- **誤検知や運用テスト時は一時的にアクションを「Challenge（認証ページ）」や「Log」に変更可**
- 必要なアクセス（自分の海外VPNや外部サービス等）は「Allow（許可）」条件で個別設定推奨
- ルール・式は随時メンテナンス可能（定期的な見直し推奨）

---

# WAF設定：許可

### ルール一覧

現在設定値はなにもありません。

### 設定方法

1. **CloudFlareダッシュボード** →「Security」→「WAF」→「ツール」タブ
    1. 単発で一時的なIPブロック/許可を追加したい場合、「Tools」から手動追加（例：特定IP・国単位で一時的にBlock、Allow、Challengeなど）

---

# キャッシュ除外設定（Cache Rules）

### ルール一覧

| **ルール名** | **条件（式）** | **実行内容** | **順序（優先度）** | **補足** |
| --- | --- | --- | --- | --- |
| 会員・認証ページ除外 | (http.request.full_uri wildcard "[https://waterme.jp/wp-admin/*](https://waterme.jp/wp-admin/*)") or (http.request.full_uri wildcard r"[https://waterme.jp/wp-login.php](https://waterme.jp/wp-login.php)") | **キャッシュをバイパスする** | 1（最優先） |  |

### ルール：会員・認証ページ除外

現状は管理画面・ログインページのみ除外。下記項目をOR条件で並列バイパス指定することで設定している。

- https://waterme.jp/wp-admin/*
    - 設定：完全URI - ワイルドカード - https://waterme.jp/wp-admin/*
- https://waterme.jp/wp-login.php
    - 設定：完全URI - ワイルドカード - https://waterme.jp/wp-login.php

※今後の除外予定　☟：会員ページ設計完了後

- **会員（SWPM）に関する全ページ**
    - 例：/member/* などの実会員ページ
        - ※特定階層なら/*などのワイルドカード指定可

### 設定方法

1. CloudFlareダッシュボード＞Caching＞Cache Rules
2. 「ルールを作成」から上記パスをOR条件で追加
3. 実行内容「キャッシュの対象外（バイパス）」
4. ルール名を明確に

### 補足

- 将来的な会員ページ・フォーム・ショッピングカート等も随時ルール追加。「管理・認証・会員系」は**1ルール内でOR条件でまとめて管理**推奨
- 除外範囲は「動的・認証を必要とするページ」「個人ごとに表示が変わるページ」に限定し、静的ページ・画像・記事はキャッシュOK
- **無料プランではCache Rulesは最大10個まで。（2025.7.3現在）**
- ルール名や内容は**いつでも**編集・変更可能（仮運用OK、柔軟運用）

---

# 関連履歴・備考

- 前回Xserver運用時のNS情報（ns1〜[5.xserver.jp](http://5.xserver.jp/)）は参考情報として残してある
- CloudFlareのネームサーバー情報は、将来変更の可能性があるため都度最新値を記録

---