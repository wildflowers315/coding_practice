# AIコードエージェントによるデータ・GIS解析入門

**更新日:** 2026-06-08

データ解析・地理情報（GIS）解析の Python コード例を、AI コードエージェント（Claude Code）を活用しながら学ぶリポジトリです。初学者を対象としています。

---

## セットアップ

以下の順番でインストールしてください。

0. [GitHub アカウントの作成](#0-github-アカウントの作成)
1. [Git](#1-git-のインストール)
2. [VS Code](#2-vs-code-のインストール)
3. [Node.js](#3-nodejs-のインストール)
4. [Python](#4-python-のインストール)
5. [uv（Python パッケージ管理）](#5-uv-のインストール)
6. [Claude Code](#6-claude-code-のインストール)

> **Windows ユーザーへ:** Git をインストールすると **Git Bash** が同梱されます。以降のコマンドはすべて Git Bash（またはターミナル）で実行してください。コマンドプロンプト（cmd）や PowerShell では動作が異なる場合があります。

---

### 0. GitHub アカウントの作成

[github.com](https://github.com/) にアクセスし、**Sign up** からアカウントを作成してください（無料）。

- メールアドレスは後の `git config` で設定するものと同じにすると管理がしやすくなります。
- 学生の場合は [GitHub Education](https://education.github.com/) に申請すると Pro プランが無料で利用できます。

---

### 1. Git のインストール

バージョン管理ツールです。**Windows では Git Bash も同時にインストールされます。**

| OS | 手順 |
|----|------|
| **Windows** | [git-scm.com/download/win](https://git-scm.com/download/win) からインストーラをダウンロードして実行。インストール中に「Git Bash Here」オプションを有効にしてください。 |
| **macOS** | `xcode-select --install` を実行（Xcode コマンドラインツールに含まれる）、または `brew install git` |
| **Linux** | `sudo apt install git`（Ubuntu / Debian） |

インストール確認：

```bash
git --version
```

初回のみ、ユーザー名とメールアドレスを設定します。

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

---

### 2. VS Code のインストール

Python 開発に広く使われているコードエディタ（IDE）です。

[code.visualstudio.com](https://code.visualstudio.com/) からお使いの OS 用のインストーラをダウンロードしてください。

インストール後、以下の拡張機能を追加することを推奨します（`Ctrl+Shift+X` で拡張機能パネルを開く）。

- **Python**（Microsoft 製）
- **Python Debugger**
- **Jupyter**

> **⚠️ 拡張機能のセキュリティに注意:** VS Code の拡張機能は誰でも公開できるため、悪意のある拡張機能がデータや認証情報を盗むケースが報告されています。インストールは **公式または信頼できる提供元のものに限定**してください。判断の目安として、提供元（Publisher）・ダウンロード数・レビュー・ソースコードの公開状況を確認する習慣をつけましょう。

---

### 3. Node.js のインストール

Claude Code の実行に必要です。**LTS（長期サポート）版を推奨します。**

| OS | 手順 |
|----|------|
| **Windows** | [nodejs.org](https://nodejs.org/) から LTS 版インストーラをダウンロードして実行 |
| **macOS** | `brew install node@22` または [nodejs.org](https://nodejs.org/) からインストーラをダウンロード |
| **Linux** | `sudo apt install nodejs npm`（Ubuntu / Debian）、または [NodeSource リポジトリ](https://github.com/nodesource/distributions) 経由で最新 LTS 版を取得 |

インストール確認：

```bash
node --version   # v22.x.x 以上であること
npm --version
```

---

### 4. Python のインストール

**Python 3.12 を推奨します。** 多くのパッケージとの互換性が高く、安定しています。

| OS | 手順 |
|----|------|
| **Windows** | [python.org/downloads](https://www.python.org/downloads/) からインストーラをダウンロード。インストール時に **「Add Python to PATH」にチェックを入れること**（これで `pip` も同時にインストールされます） |
| **macOS** | `brew install python@3.12` または [python.org](https://www.python.org/downloads/) からインストーラをダウンロード |
| **Linux** | `sudo apt install python3.12 python3.12-venv python3-pip`（Ubuntu / Debian） |

インストール確認：

```bash
# Windows（Git Bash）/ macOS / Linux
python --version     # または python3 --version
pip --version
```

> **pip について:** Python の標準パッケージインストーラです。Windows では Python インストーラの「Add Python to PATH」を有効にすれば自動で入ります。このリポジトリでは pip の代わりに高速な **uv** を使いますが、pip 自体は Python に付属しているので追加インストールは不要です。

---

### 5. uv のインストール

Python の仮想環境作成とパッケージインストールを高速に行えるツールです。`pip` や `conda` の代替として使います。

```bash
# macOS / Linux / Git Bash（Windows）
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```powershell
# Windows（PowerShell の場合）
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

ターミナルを再起動して確認します。

```bash
uv --version
```

#### Python バージョンの管理

uv は Python 自体のインストールと管理も行えます。OS に Python を別途インストールしなくても、uv だけで完結させることができます。

```bash
# 利用可能・インストール済みの Python バージョン一覧
uv python list

# Python 3.12 をインストール
uv python install 3.12
```

#### プロジェクトのセットアップ（推奨ワークフロー）

```bash
# プロジェクトフォルダに移動してプロジェクトを初期化
# （pyproject.toml と .python-version が生成される）
uv init

# requirements.txt からパッケージをまとめて追加
# （pyproject.toml と uv.lock に自動記録される）
uv add -r requirements.txt

# または個別に追加
uv add numpy pandas geopandas

# スクリプトを実行（venv の有効化不要）
uv run python scripts/sample.py
uv run jupyter notebook

# チームメンバーが環境を再現するとき
uv sync
```

> **`uv.lock` について:** `uv add` を実行すると `uv.lock` が自動生成されます。このファイルをリポジトリに含めることで、誰がどの環境で `uv sync` を実行しても完全に同じパッケージバージョンを再現できます。

#### レガシーワークフロー（参考）

`pip` に慣れている場合や既存スクリプトとの互換性が必要な場合は、以下の方法も使えます。

```bash
# 仮想環境を手動作成・有効化してから pip 互換コマンドで操作
uv venv --python 3.12
source .venv/bin/activate          # macOS / Linux / Git Bash
.venv\Scripts\activate.bat         # Windows コマンドプロンプト

uv pip install -r requirements.txt
deactivate
```

---

### 6. Claude Code のインストール

Anthropic が提供する AI コーディングアシスタントの CLI ツールです。ターミナル上でコードの作成・説明・デバッグを AI と対話しながら行えます。

```bash
npm install -g @anthropic-ai/claude-code
```

インストール後、プロジェクトのフォルダに移動して起動します。

```bash
cd your-project
claude
```

初回起動時に Anthropic アカウントへのサインインを求められます。画面の指示に従ってください。

> VS Code・JetBrains IDE の拡張機能、およびデスクトップアプリ版も利用できます。詳細は [claude.ai/code](https://claude.ai/code) を参照してください。

---

## このリポジトリをフォークする

フォークとは、このリポジトリを自分の GitHub アカウントにコピーすることです。自分のコピーに変更を加えて、プルリクエスト（Pull Request）でこのリポジトリに提案を送ることができます。

**1. フォークする**

このリポジトリのページ右上の **Fork** ボタンをクリックして、自分のアカウントにコピーします。

**2. フォークしたリポジトリをクローンする**

```bash
# <your-username> を自分の GitHub ユーザー名に置き換える
git clone https://github.com/<your-username>/coding_practice.git
cd coding_practice
```

**3. 上流リポジトリを登録する**

```bash
git remote add upstream https://github.com/wildflowers315/coding_practice.git
```

これにより、元のリポジトリの更新を自分のフォークに取り込めるようになります。

```bash
# 元リポジトリの最新変更を取得して取り込む
git fetch upstream
git merge upstream/main
```

**4. プルリクエストを送る**

変更を加えてコミット・プッシュした後、GitHub 上の自分のフォークページで **Contribute → Open pull request** をクリックすることで、このリポジトリへの変更提案を送れます。

---

## ドキュメント構成

```
docs/
└── 00_env/
    ├── 01_python_env.md   # Python 環境セットアップ詳細・IDE 使い方・GIS ライブラリ入門
    └── 02_claude_code.md  # Claude Code の使い方・コンテキスト・モデル・セッション再開

scripts/
└── sample.py              # 各パッケージ（numpy・pandas・requests・openpyxl・pptx・pymupdf4llm）の使用例
```

