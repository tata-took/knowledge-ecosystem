---
name: get-rss-sources
description: |
  ユーザーが「/get-rss_sources」と入力したとき、
  またはRSSソース一覧を確認・取得・表示したいとき、
  またはrss_sources.ymlの内容を参照・確認・一覧表示したいとき、
  またはRSSフィードの設定を見たいと言ったときに必ず使うスキル。
  GitリポジトリルートはC:\Users\拓哉\.cursor\knowledge-ecosystem（knowledge-ecosystemが親）。
  ./news/config/rss_sources.yml を読み込んで内容を表示する。
---

# get-rss-sources スキル

`tata-took/knowledge-ecosystem` リポジトリの `./news/config/rss_sources.yml` を取得・表示するスキル。

## 固定パラメータ

| パラメータ | 値 |
|---|---|
| owner | `tata-took` |
| repo | `knowledge-ecosystem` |
| branch | `main` |
| ファイルパス（GitHub相対） | `news/config/rss_sources.yml` |

> **パス規則**
> - Gitルート = `knowledge-ecosystem/`（親ディレクトリ）
> - news以下を指定するときは `./news/...` → GitHub上は `news/...`
> - 例: `./news/config/rss_sources.yml` → `news/config/rss_sources.yml`

---

## 手順

### Step 1: GitHubからファイルを取得する

```
docker-mcp-toolkit:get_file_contents を呼び出す

  owner  : tata-took
  repo   : knowledge-ecosystem
  branch : main
  path   : news/config/rss_sources.yml
```

### Step 2: 内容をデコードして表示する

- `get_file_contents` の返り値は Base64 エンコードされている場合がある
- `content` フィールドを Base64 デコードしてYAMLテキストとして表示する

### Step 3: ユーザーへの出力形式

取得成功時：

```
📋 rss_sources.yml の内容
パス: news/config/rss_sources.yml
リポジトリ: tata-took/knowledge-ecosystem @ main

---
（YAMLの中身をそのまま表示）
---

✅ 取得完了
```

エラー時：
```
❌ 取得失敗
原因: <エラーメッセージ>
対処: Docker MCP Toolkitが起動しているか確認してください
```

---

## 注意点・落とし穴

- **Docker MCP Toolkit がタイムアウトする場合** → ユーザーに再起動を依頼する
- **パスは必ず `news/config/rss_sources.yml`（スラッシュ区切り）** → Windowsの `\` はGitHub APIに渡さない
- **ファイルが存在しない場合** → `news/config/` 配下のファイル一覧を `get_file_contents` でディレクトリ指定して確認する
- **Base64デコード** → JavaScriptの `atob()` またはPythonの `base64.b64decode()` を使う。ツール側で自動デコードされる場合はそのまま使用可
