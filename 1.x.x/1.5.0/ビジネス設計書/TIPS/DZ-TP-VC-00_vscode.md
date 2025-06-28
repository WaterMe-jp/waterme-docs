# vscode

---

### 当ページメタ情報

ページID：DZ-TP-VC-00

ページ名：vscode

ファイル名：DZ-TP-VC-00_vscode.md

ページ目的：webコンテンツビジネスの運用面において便宜上、よく使うvscodeTIPSを保管するページ。

ページ構成：ビジネス設計書＞TIPS＞vscode

最終更新：2025/6/24

---

# Xserver接続：Remote - SSH

## ✅ 事前準備

1. **VSCode に拡張機能「Remote - SSH」をインストール**
    - 拡張機能 → 検索 → `Remote - SSH` → インストール
2. **エックスサーバー側で SSH 接続を有効にする**
    - サーバーパネルにログイン
    - 「SSH設定」→「ON」に変更
    - 秘密鍵認証方式が必要（パスワード認証は不可）
3. **秘密鍵（.ppk 形式）を作成する**
    - サーバーパネルの「SSH設定」から秘密鍵（.ppk形式）をダウンロード
    - 形式が OpenSSH でない場合は変換が必要（→ `puttygen` など使用）

---

## 🛠 SSH 設定ファイルの準備

1. **SSH設定ファイルの場所**
    - Windows：`C:\\Users\\ユーザー名\\.ssh\\config`
    - macOS/Linux：`~/.ssh/config`
2. **設定ファイルに以下を追記（例）**
    
    ```jsx
    Host xserver
    HostName サーバーIPアドレス
    User ユーザー名（例：xs999999）
    Port 10022
    IdentityFile ~/.ssh/xserver.key  # ← 秘密鍵のフルパス
    
    ※ `Port 10022` はエックスサーバーで指定されているポート番号です。
    ```
    

---

## 🚀 VSCode から接続

1. `F1` を押すか `Ctrl+Shift+P` でコマンドパレットを開く
2. 「Remote-SSH: Connect to Host」→ `xserver` を選択
3. 初回はホスト鍵の確認が表示されるので「Continue」を選択
4. 接続後、VSCodeの左下に「SSH: xserver」と表示されれば接続成功

---

## 📁 ファイル操作とログ確認

- 接続先の `/home/ユーザー名/log/error_log` などを開く
- 右クリック → 「Open」や「Download」でログ確認可能

---

## ✅ 注意点

- `xserver.key` には適切なパーミッション（例：`600`）を設定してください
- `chmod 600 ~/.ssh/xserver.key` でOK
- エラーが出る場合、鍵ファイルの形式が OpenSSH であることを確認してください

---