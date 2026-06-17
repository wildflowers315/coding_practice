# 03. VS Code — Python 開発の使い方

> **参考:**
> - [VS Code 公式ドキュメント（Python）](https://code.visualstudio.com/docs/python/python-tutorial)
> - [VS Code キーボードショートカット（Windows）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
> - [VS Code キーボードショートカット（macOS）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)

---

## 学習目標

- VS Code に Python 拡張機能を導入できる
- ターミナルのデフォルトシェルを Bash に変更できる
- コードを1行ずつ・選択範囲で実行できる
- VS Code 内で Jupyter Notebook を使える
- デバッガでコードの動きを追える

---

## 1. 拡張機能のインストール

`Ctrl+Shift+X`（macOS: `Cmd+Shift+X`）で拡張機能パネルを開き、以下をインストールします。

| 拡張機能 | 提供元 | 用途 |
|---------|--------|------|
| **Python** | Microsoft | コード補完・lint・インタープリタ選択 |
| **Python Debugger** | Microsoft | ブレークポイントデバッグ |
| **Jupyter** | Microsoft | VS Code 内で `.ipynb` を実行 |

> **⚠️ 拡張機能のセキュリティ:** 拡張機能は誰でも公開できます。インストールは**公式または信頼できる提供元のものに限定**し、提供元・ダウンロード数・レビューを確認する習慣をつけてください。

---

## 2. ターミナルを Bash に設定する

VS Code のターミナルは `Ctrl+Shift+`` `（macOS: `Ctrl+`` `）で開きます。
デフォルトシェルを Bash に変更するには以下の手順を行います。

### Windows（Git Bash を使う）

1. `Ctrl+Shift+P` → `Terminal: Select Default Profile` を入力して選択
2. 一覧から **Git Bash** を選択

> Git Bash が表示されない場合は Git がインストールされていない可能性があります（README の手順1を参照）。

### macOS / Linux

デフォルトで Bash または Zsh が使われます。Bash に固定したい場合:

1. `Ctrl+Shift+P` → `Terminal: Select Default Profile`
2. **bash** を選択

### 設定ファイルで直接変更する方法（共通）

`Ctrl+Shift+P` → `Preferences: Open User Settings (JSON)` を開き、以下を追加します。

```json
// Windows（Git Bash の場合）
"terminal.integrated.defaultProfile.windows": "Git Bash",

// macOS
"terminal.integrated.defaultProfile.osx": "bash",

// Linux
"terminal.integrated.defaultProfile.linux": "bash"
```

---

## 3. Python インタープリタの設定

プロジェクトの仮想環境（`.venv`）を VS Code に認識させます。

1. `Ctrl+Shift+P` → `Python: Select Interpreter` を入力して選択
2. 一覧から **`.venv/bin/python`**（Windows: `.venv\Scripts\python.exe`）を選択

> `.venv` が表示されない場合は、先に `uv venv` または `uv sync` を実行して仮想環境を作成してください。

画面下のステータスバーにインタープリタが表示されます。ここをクリックして変更することもできます。

---

## 4. コードの実行方法

### 4-1. ファイル全体を実行する

右上の ▷ ボタン、または `Shift+F5` でファイル全体を実行します。
ターミナルに結果が出力されます。

```bash
# ターミナルから直接実行する場合（推奨）
uv run python scripts/sample.py
```

### 4-2. 行・選択範囲を実行する（Run Selection）

コードを1行ずつ、またはブロック単位で実行できます。

| 操作 | ショートカット |
|------|--------------|
| カーソル行 or 選択範囲を実行 | `Shift+Enter` |
| 選択してコマンドパレットから実行 | `Ctrl+Shift+P` → `Python: Run Selection/Line in Python Terminal` |

実行結果はターミナルの **Python REPL**（対話モード）に出力されます。
変数の中身を確認しながら少しずつコードを試したいときに便利です。

```python
# 例：この行にカーソルを置いて Shift+Enter
import numpy as np
x = np.array([1, 2, 3, 4, 5])
print(x.mean())   # <- ここで Shift+Enter → 3.0 が出力される
```

### 4-3. Interactive Window（インタラクティブウィンドウ）

Jupyter Notebook に似た対話環境を VS Code 内で使えます。

**起動方法:**
`Ctrl+Shift+P` → `Python: Start Native Python REPL` または `Jupyter: Create Interactive Window`

**セル区切りを使う（`.py` ファイル内）:**

```python
# %% と書くとセルの区切りになります
# %%
import pandas as pd
df = pd.read_csv('data/sample.csv')

# %%
print(df.head())
```

各セルの上に `Run Cell` ボタンが現れ、クリックで実行できます。
グラフや地図もインラインで表示されます。

---

## 5. Jupyter Notebook の使い方

### 5-1. パッケージのインストール

```bash
uv add jupyter
```

### 5-2. VS Code で Notebook を開く・新規作成する

**既存の `.ipynb` を開く:**
エクスプローラー（左サイドバー）から `.ipynb` ファイルをクリックするだけで VS Code 内で開けます。

**新規作成:**
`Ctrl+Shift+P` → `Jupyter: Create New Jupyter Notebook` → ファイルを `.ipynb` で保存

### 5-3. カーネル（Python 環境）の選択

Notebook 右上の **カーネル選択** をクリック → **Python Environments** → `.venv` を選択します。

> カーネルが `.venv` を指していないと、インストールしたパッケージが使えません。必ず確認してください。

### 5-4. セルの操作

| 操作 | ショートカット |
|------|--------------|
| セルを実行して次へ進む | `Shift+Enter` |
| セルを実行してその場に留まる | `Ctrl+Enter` |
| 上にセルを追加 | コマンドモードで `A` |
| 下にセルを追加 | コマンドモードで `B` |
| セルを削除 | コマンドモードで `DD`（D を2回） |
| コードセル ↔ Markdown セルの切り替え | コマンドモードで `M` / `Y` |
| コマンドモードに入る | `Esc` |
| 編集モードに入る | `Enter` |

> セルの外側（青いバー）をクリックするとコマンドモードになります。

### 5-5. すべてのセルを実行する

ツールバーの **Run All**（▷▷）ボタン、または `Ctrl+Shift+P` → `Jupyter: Run All Cells`

### 5-6. ブラウザで Jupyter を使う（従来の方法）

VS Code を使わずブラウザ上で Jupyter を使いたい場合:

```bash
# 仮想環境を有効化してから起動
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows（Git Bash）

jupyter notebook
```

ブラウザが自動で開きます。`New → Python 3` で新しいノートブックを作成できます。
終了するにはターミナルで `Ctrl+C` を押してください。

> **Git との相性について:** Notebook は出力（グラフ・テキスト）も `.ipynb` に保存されるため、コミットのたびに差分が大きくなります。本番コードは `.py` で管理し、探索・デモ用にノートブックを使うのがベストプラクティスです。

---

## 6. デバッガの使い方

「どこでエラーが起きているかわからない」「変数の中身を途中で確認したい」ときに使います。

### 6-1. ブレークポイントを設定する

行番号の左端をクリックすると赤い点（ブレークポイント）が付きます。
そこでコードの実行が一時停止します。

### 6-2. デバッグを開始する

`F5` を押すか、右上の ▷ 横のドロップダウン → **Python Debugger: Debug Python File** を選択します。

### 6-3. ステップ実行

| ショートカット | 動作 |
|-------------|------|
| `F5` | 次のブレークポイントまで実行 |
| `F10` | 次の行へ（関数の中には入らない） |
| `F11` | 関数の中に入る |
| `Shift+F11` | 関数から出る |
| `Shift+F5` | デバッグを停止 |

### 6-4. 変数を確認する

一時停止中に左サイドバーの **VARIABLES** パネルで現在のすべての変数と値を確認できます。
また、コード上で変数名にマウスを乗せるとポップアップで値が表示されます。

---

## 7. よく使うショートカット まとめ

| ショートカット | 機能 |
|-------------|------|
| `Ctrl+Shift+P` | コマンドパレット（すべての機能にアクセス） |
| `Ctrl+P` | ファイルをすばやく開く |
| `Ctrl+`` ` | ターミナルを開く / 閉じる |
| `Ctrl+Shift+X` | 拡張機能パネルを開く |
| `Ctrl+/` | 行をコメントアウト / 解除 |
| `Shift+Enter` | 選択行を実行（Python REPL）/ セルを実行（Notebook） |
| `Ctrl+クリック` | 関数・変数の定義へジャンプ |
| `F2` | 変数・関数名を一括リネーム |
| `Ctrl+Z` | 元に戻す |
| `Ctrl+Shift+Z` | やり直す |
| `Ctrl+S` | 上書き保存 |

> macOS では `Ctrl` の代わりに `Cmd` を使います（例: `Cmd+Shift+P`）。

---

## 参考リンク

- [VS Code 公式ドキュメント — Python チュートリアル](https://code.visualstudio.com/docs/python/python-tutorial)
- [VS Code 公式ドキュメント — Jupyter Notebook](https://code.visualstudio.com/docs/datascience/jupyter-notebooks)
- [VS Code 公式ドキュメント — デバッガ](https://code.visualstudio.com/docs/python/debugging)
- [VS Code キーボードショートカット（Windows）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
- [VS Code キーボードショートカット（macOS）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
- [VS Code キーボードショートカット（Linux）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
