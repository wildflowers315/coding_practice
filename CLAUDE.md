# CLAUDE.md — coding_practice

## プロジェクト概要

AIコードエージェント（Claude Code）を活用しながら、データ解析・GIS解析の Python コード例を学ぶ入門リポジトリ。対象：初学者。

## 環境

- Python 3.12（`uv` で管理）
- パッケージ管理: `uv add` / `uv sync`
- スクリプト実行: `uv run python <script>`（venv の手動有効化不要）

## よく使うコマンド

```bash
# スクリプト実行
uv run python scripts/sample.py

# パッケージ追加
uv add <package>

# 環境の再現
uv sync
```

## ディレクトリ構成

```
docs/00_env/   # 環境セットアップ・Claude Code 使い方ドキュメント
scripts/       # サンプルスクリプト
data/          # ダウンロードしたデータ（gitignore 済み）
output/        # 生成ファイル（gitignore 済み）
```

## 規約

- コメントは日本語で書く
- 出力ファイルは `output/` に保存する
- 認証情報は `.env` に書き、直接コードに書かない
