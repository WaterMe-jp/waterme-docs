# GitHub管理

---

### 当ページメタ情報

ページID：DZ-MG-AL-01

ページ名：GitHub管理

ファイル名：DZ-MG-AL-01_GitHub管理.md

ページ目的：全体データのGitHub上管理方法・運用面についてのまとめ。

ページ構成：ビジネス設計書＞データ管理＞全体＞GitHub管理

最終更新：2025/6/24

---

# ◆ リポジトリ

- GitHubは、WaterMeプロジェクトにおける設計・構築・資産管理の**透明性と再現性を高める目的**で使用。
- WaterMeプロジェクトにおける重要データを総括的に安全に保管する。
- 主な管理対象リポジトリ：

| **リポジトリ名** | **用途概要** |
| --- | --- |
| `waterme-public` | WordPress本番コード（public_html）を管理 |
| `waterme-assets` | 使用画像・QRコードなど静的アセットの保管 |
| `waterme-docs` | Notion書き出し・企画書・設計書の保存 |
| `waterme-templates` | LPや構成テンプレート（HTML等）の管理 |
| `waterme-scripts` | 自動化スクリプトや運用補助コードの保管 |
- 命名ルールは `waterme-◯◯` で統一し、機能・中身を明示。

---

# ◆ バックアップ

- 基本は **変更都度Push** によりGitHubへ保存。
- リポジトリには `.gitignore` を適切に設定し、不要ファイルは追跡対象外に。
- 不要となったリポジトリも削除せず、**「Archive this repository」** によって保管。

---

# ◆ .gitignore

TODO:　それぞれ記載・.gitignoreのコードをそのまま貼ること。

### `waterme-public`

```

```

### `waterme-assets`

```

```

### `waterme-docs`

```

```

### `waterme-templates`

```

```

### `waterme-scripts`

```

```

---

# ◆ 公開範囲

- すべてのリポジトリは **原則Private** で作成・運用。
- 公開が必要な場合は個別判断のうえ、目的を明示して一部のみ公開。
- **機密情報（パスワード、APIキー等）は一切コミット禁止**。

---

# ◆ Push/Pull

- `wp-content/uploads/` は原則 `.gitignore` に含めるが、特定ディレクトリ（例：`design_export/`）のみ例外として追跡。
- WordPressコアファイル（`wp-admin`, `wp-includes` 等）は基本除外。
- `.gitattributes` にて改行コードの統一（例：`text=auto`）を設定。
- `README.md` に**構造・目的・注意事項**を記載し、Pullした人がすぐ理解できるよう配慮。

---

# ◆ 検討点

- `scripts/` 内にCI/CD系の自動化スクリプトを整理。
- `tags`や`release`でのバージョン管理導入。
- GitHub Actions等による継続的インテグレーション（CI）の導入。

---