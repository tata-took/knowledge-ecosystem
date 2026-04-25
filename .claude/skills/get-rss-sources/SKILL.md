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

ユーザーが `/get-rss_sources` またはRSSソース設定の確認を求めた際に、
`./news/config/rss_sources.yml` を読み込んで内容を返す。

## リポジトリ構造

```
knowledge-ecosystem/          ← Git root（親）
└── news/                     ← ./news で参照
    └── config/
        └── rss_sources.yml   ← このファイルを読む
```

## 手順

1. `docker-mcp-toolkit:read_file` を使って以下のパスを読み込む：

```
/C/mcp/knowledge-ecosystem/news/config/rss_sources.yml
```

2. 読み込んだ内容をカテゴリ別に整理して表示する

3. 各ソースの以下の情報をまとめて返す：
   - `name`（ソース名）
   - `url`（RSS URL）
   - `fetch_type`（rss / html_scrape / search_fallback）
   - `category`（カテゴリ）
   - `status`（動作確認状況）

## 出力フォーマット

```
📡 RSS Sources 一覧（./news/config/rss_sources.yml）
取得日時：YYYY-MM-DD

【カテゴリ名】
| # | 名前 | fetch_type | status |
|---|------|------------|--------|
| 1 | Zenn | rss        | ✅     |
...

合計：XX ソース
✅ 動作確認済み：XX件
未確認：XX件
❌ 要対応：XX件
```

## パス解決ルール

| 環境 | パス |
|------|------|
| Docker MCP Toolkit（filesystem） | `/C/mcp/knowledge-ecosystem/news/config/rss_sources.yml` |
| Windows ローカル参照 | `C:\Users\拓哉\.cursor\knowledge-ecosystem\news\config\rss_sources.yml` |
| Git相対パス | `./news/config/rss_sources.yml` |
| GitHub API | `owner=tata-took / repo=knowledge-ecosystem / path=news/config/rss_sources.yml` |

## 注意点

- Git rootは `knowledge-ecosystem`（親ディレクトリ）
- `./news` = `knowledge-ecosystem/news`
- filesystemアクセスは `/C/mcp/knowledge-ecosystem` がallowed directory
- ファイルが存在しない場合はGitHub API（`docker-mcp-toolkit:get_file_contents`）でフォールバック
