# 公開時ルール

---

### 当ページメタ情報

ページID：DZ-MG-AL-02

ページ名：公開時ルール

ファイル名：DZ-MG-AL-02_公開時ルール.md

ページ目的：設計書や運用記録などを **外部公開する際のガイドライン** を定め、機密保持と透明性の両立を目指す。

ページ構成：ビジネス設計書＞データ管理＞全体＞公開時ルール

最終更新：2025/6/19

変更点：ー

---

# 基本ルール

### 📄 公開対象の想定

- 検証結果、仕様概要、リリース記録など
- ChatGPTにレビューさせる目的で一時的に公開する設計書

### 🔐 公開形式のルール

- **Google Drive**：リンクを知っているユーザーのみに制限／DL・編集不可設定
- **自サイト上のhtmlページ**：必要に応じてパスワード保護 or ロボット制御
- **PDF化**：ファイル改変防止／閲覧専用／Notionとの併用可

### ❌ 非公開とすべきもの

- ログイン情報、APIキー、バックヤードURL等
- セキュリティ設定やアクセス制御に関する記述

### 🔁 公開期間・更新

- ChatGPTレビュー用の公開ページは **レビュー終了後に非公開化 or 削除**
- 外部レビューを再依頼する際は **都度更新・再公開の上、URL変更も検討**

### ✅ その他

- 公開状態で不安がある場合は「パスワード付きHTML」＋「一時公開」が原則
- 公開したURLやファイルはNotionで一覧管理／履歴を追えるようにする

# Notion・PDF・GitHub等「レビュー依頼用に外部共有する際の手順」

- **Notion**：
    - ページ右上の「共有」から「Web上でリンクを知る全ユーザー（編集 or 閲覧）」を選択。
    - URLを取得して、**レビュー依頼相手に直接共有**。
    - 編集権限は通常付与せず、「閲覧専用」リンク推奨。
- **PDF**：
    - Google Drive等でアップロードし、「リンクを知っている人のみ閲覧可能」に設定。
    - 添付欄がないフォーム・メールではURL共有で代替。
    - PDF右下のページ番号や目次でナビゲーションしやすく整理。
- **GitHub（Public/Private）**：
    - Publicにしてよい情報であれば`waterme-docs`などに配置し、URL共有。
    - Privateの場合は、レビュアーのGitHubアカウントに招待し、**Collaboratorとしてアクセス権を付与**。

# 公開設定・パスワード管理の方針

- **パスワード設定の有無**：
    - 「公開状態で誰でも閲覧可」と「パスワード制限付き公開」の**2段階で使い分け**。
    - 設計書などセンシティブな内容は原則パスワード保護。
- **パスワード共有方法**：
    - ChatGPTや関係者に個別伝達（DM、メール、専用チャット）。
    - パスワードを頻繁に変更せず、**固定化＋最小限共有**が基本方針。
- **公開解除の管理**：
    - 一時的に公開設定を解除した場合、**終了後すぐに再制限をかける習慣をルール化**。

# URLの有効期限／公開範囲のガイドライン

- **一時的レビュー用途**の場合：
    - 公開URLは「〇日間限定」などと有効期間を明示（メモ上にも記載）。
    - 使用終了後、公開設定を**Draft・パスワード付きに戻す or 削除**。
- **恒常的に公開したい設計書等**：
    - 内容を吟味して、**機密情報を含まないよう編集したうえで公開**。
    - Googleインデックス対策として `noindex` 設定を活用（SEOプラグインにて対応）。
- **共有範囲**：
    - 「ChatGPTのみ」「外部レビュアーへ個別」など目的ごとに共有先を明確化。
    - 閲覧記録や履歴が必要な場合は、GitHubやNotionで共有し管理ログを保持。