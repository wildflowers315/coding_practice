# 01. Python 環境セットアップ

> **原典（本ドキュメントはこれら2つのチュートリアルを統合・再構成しています）:**
>
> - [Python Environments — WUR GeoScripting](https://geoscripting-wur.github.io/pythonEnvs/)
> - [Tutorial 9: Python Programming — WUR GeoScripting](https://geoscripting-wur.github.io/PythonProgramming/)
>
> 著者: Arno Timmer, Jan Verbesselt, Jorge Mendes de Jesus, Aldo Bergsma, Johannes Eberenz,  
> Dainius Masiliunas, David Swinkels, Judith Verstegen, Corné Vreugdenhil  
>
> パッケージ管理を `conda` / `mamba` から **`uv`** に置き換えています。コード・構成は原典に準拠。

---

## 学習目標

- Python と仮想環境の概念を理解できる
- `uv` を使って仮想環境を作成・管理できる
- VS Code でコードを書き、デバッグできる
- Jupyter Notebook でインタラクティブに実行できる
- Python の基本構文を理解できる
- Matplotlib・Cartopy・GeoPandas・Rasterio で地図を描ける

---

## 1. Python とは

Python は無料・オープンソース・クロスプラットフォームのプログラミング言語で、多くのランキングで世界一の人気を誇ります。オランダ人コンピュータ科学者 Guido van Rossum が「プログラマーの時間を大切にする」思想で設計したため、読み書きがしやすい言語です。

大規模なコミュニティにより、地理解析・データ処理・機械学習など多数の高品質パッケージが公開されています。

### 主要パッケージ一覧

| カテゴリ | パッケージ | 用途 |
|----------|-----------|------|
| 地理解析 | GeoPandas | ベクターデータ処理 |
| 地理解析 | Rasterio | ラスターデータ処理 |
| 地理解析 | GDAL/OGR | ベクター・ラスター処理 |
| データ処理 | Pandas | データフレーム・データ分析 |
| データ処理 | NumPy | 科学計算 |
| 可視化 | Matplotlib | 汎用グラフ描画 |
| 可視化 | Cartopy | 地図投影・空間可視化 |
| 可視化 | Folium | インタラクティブ地図 |
| 機械学習 | scikit-learn | 機械学習 |
| 機械学習 | PyTorch | 深層学習 |

---

## 2. 仮想環境とパッケージ管理

パッケージはそれぞれ依存関係（他パッケージへの依存）を持ちます。プロジェクトごとに**仮想環境**を作ると、依存関係の衝突を防ぎ、環境を他の人と共有できます。

原典では `Mamba`（`Conda` の高速実装）を使用していますが、本ドキュメントでは **`uv`** を採用します。`uv` は Rust 製の高速なパッケージ・仮想環境管理ツールで、Python 自体のインストールからパッケージ管理・スクリプト実行まで一括して行えます。

### 2-1. uv のインストール

```bash
# Linux / macOS
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows（PowerShell）
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

ターミナルを再起動して確認します。

```bash
uv --version
```

### 2-2. Python バージョンの管理（uv python）

uv は OS とは独立して Python 自体を管理できます。

```bash
# 利用可能・インストール済みのバージョンを一覧表示
uv python list

# Python 3.12 をインストール
uv python install 3.12

# プロジェクトで使う Python バージョンを固定（.python-version ファイルを生成）
uv python pin 3.12
```

### 2-3. プロジェクトの初期化（uv init）

```bash
# プロジェクトフォルダに移動してプロジェクトを初期化
cd your-project
uv init
```

`pyproject.toml`（プロジェクト設定）と `.python-version` が生成されます。

### 2-4. パッケージの追加（uv add）

```bash
# 個別に追加（pyproject.toml と uv.lock に自動記録）
uv add numpy pandas matplotlib

# requirements.txt からまとめて追加
uv add -r requirements.txt

# パッケージを削除
uv remove folium

# インストール済みパッケージ一覧を確認
uv pip list
```

> **`uv.lock` について:** `uv add` を実行すると `uv.lock` が自動生成されます。このファイルをリポジトリに含めることで、`uv sync` を実行するだけで誰でも完全に同じ環境を再現できます。

### 2-5. スクリプトの実行（uv run）

`uv run` を使うと仮想環境を手動で有効化せずにスクリプトを実行できます。

```bash
# Python スクリプトを実行
uv run python scripts/sample.py

# Jupyter Notebook を起動
uv run jupyter notebook

# 環境を再現してから実行（uv.lock をもとに自動で sync）
uv sync
uv run python scripts/sample.py
```

### 2-6. レガシーワークフロー（参考）

既存スクリプトとの互換性が必要な場合や `pip` に慣れている場合は、以下の方法も使えます。

```bash
# 仮想環境を手動作成
uv venv --python 3.12

# 有効化（Linux / macOS）
source .venv/bin/activate

# 有効化（Windows）
.venv\Scripts\activate

# パッケージをインストール
uv pip install -r requirements.txt

# 終了
deactivate
```

---

## 3. ターミナルからスクリプトを実行する

### uv run（推奨）

仮想環境を有効化しなくても `uv run` でスクリプトを実行できます。プロジェクトの `.venv` を自動で参照します。

```bash
# スクリプトを実行
uv run python scripts/sample.py

# 対話モード（REPL）で起動
uv run python

# Jupyter を起動
uv run jupyter notebook
```

バージョン確認：

```python
import sys
print(f'Python バージョン: {sys.version}')
exit()
```

### 仮想環境を有効化して実行（従来の方法）

```bash
source .venv/bin/activate      # Linux / macOS
.venv\Scripts\activate         # Windows
python scripts/sample.py
deactivate
```

> IDE から実行するより、ターミナルから `uv run` で実行する方がメモリ汚染のリスクが低く、再現性の確認に適しています。

---

## 4. エディタと IDE

### 4-1. Visual Studio Code（推奨）

VS Code は最も広く使われているIDEのひとつで、Python・GIS開発に必要な機能をすべて備えています。

#### 拡張機能のインストール

左サイドバーの拡張機能アイコン（または `Ctrl+Shift+X`）から以下をインストールします。

- **Python**（Microsoft製）
- **Python Debugger**
- **Jupyter**

#### プロジェクト構造の例

```
MyProject/
├── requirements.txt
├── main.py
└── mypackage/
    └── __init__.py
```

フォルダ単位で開くと（`File → Open Folder`）、インタープリタの設定・検索置換・import ナビゲーションなど、プロジェクト全体の機能が使えます。

#### インタープリタの設定

`Ctrl+Shift+P` → **Python: Select Interpreter** → `.venv/bin/python`（Windows: `.venv\Scripts\python.exe`）を選択します。

#### デバッガー

実行ボタン横のドロップダウン → **Python Debugger: Debug Python File** を選択すると、ブレークポイントで一時停止してコードの状態を検査できます。

| ショートカット | 動作 |
|--------------|------|
| `F5` | 次のブレークポイントまで実行 |
| `F10` | 次の行へ（関数に入らない） |
| `F11` | 関数の中に入る |
| `Shift+F11` | 関数から出る |
| `Ctrl+Shift+F5` | 再起動 |
| `Shift+F5` | 停止 |

#### REPL（対話モード）

`Ctrl+Shift+P` → **Python: Start Native Python REPL** でノートブック形式の対話環境が起動します。コードをセル単位で実行でき、グラフや地図をインライン表示できます。

```python
import folium
m = folium.Map(location=[35.6895, 139.6917], zoom_start=12)  # 東京
m
```

#### Git ソース管理

左サイドバーのブランチアイコン（または `Ctrl+Shift+G`）で Git 操作が視覚的に行えます。

| 操作 | 方法 |
|------|------|
| `git status` | Changes セクションでファイル状態を確認（U=未追跡, M=変更, A=ステージ済み） |
| `git add` | ファイル横の `+` アイコン |
| `git reset` | ステージ済みファイル横の `-` アイコン |
| `git commit` | 上部の入力欄にメッセージを入力してチェックマークをクリック |
| `git push / pull` | `...`（More Actions）メニュー |

マージコンフリクトが発生した場合は、コンフリクトファイルの **Resolve in Merge Editor** ボタンで3ウェイマージエディタを使って視覚的に解決できます。

詳細: [VS Code Source Control ドキュメント](https://code.visualstudio.com/docs/sourcecontrol/overview)

#### キーボードショートカット（よく使うもの）

| ショートカット | 機能 |
|-------------|------|
| `Ctrl+Shift+P` | コマンドパレットを開く |
| `Ctrl+P` | ファイル検索 |
| `Ctrl+クリック` | 関数定義へジャンプ |
| `Ctrl+.` | クイックフィックスの提案 |
| `Ctrl+Shift+`` ` | ターミナルを開く |

### 4-2. Jupyter Notebook

Jupyter はコードと可視化を1つのドキュメントにまとめられる実行環境です。探索的データ解析やチュートリアル作成に向いています。

```bash
# インストール（uv で）
uv pip install jupyter

# 起動（仮想環境を有効化した状態で）
jupyter notebook
```

ブラウザが開き、ファイルブラウザが表示されます。**New → Python 3** で新規ノートブック（`.ipynb` ファイル）を作成します。

> Jupyter Notebook は Git との相性がよくありません（出力が保存されるため差分が大きくなる）。本番コードには `.py` スクリプトを使い、探索・デモ用にノートブックを使うのがよい慣習です。

### 4-3. Google Colab

Google が提供するクラウド上の Jupyter 実行環境です。インストール不要でブラウザだけで動きます。

- URL: [colab.research.google.com](https://colab.research.google.com/)
- パッケージのインストール: `!pip install folium`

研究コミュニティで広く使われており、公開されたノートブックを実行する際に役立ちます。

### 4-4. Spyder

R の RStudio に似た軽量 IDE です。変数ビューア・コードエディタ・コンソール・グラフペインが1つの画面に並びます。

```bash
uv pip install spyder
spyder
```

主なショートカット: `F5`（実行）、`Ctrl+S`（保存）、`Ctrl+1`（コメントアウト）

---

## 5. Python 基礎リフレッシュ

### 変数と基本データ型

```python
age = 25          # int（整数）
height = 1.75     # float（浮動小数点）
name = "山田 太郎"  # str（文字列）
is_student = True # bool（真偽値）

# f文字列で整形して出力
print(f'{name} は {age} 歳、身長 {height} m です。')
```

### 四則演算

```python
a, b = 10, 5
print(a + b)   # 15
print(a - b)   # 5
print(a * b)   # 50
print(a / b)   # 2.0
print(a % b)   # 0（余り）
print(a ** b)  # 100000（べき乗）
```

### 条件分岐

```python
x = 15
if x > 10:
    print("x は 10 より大きい")
elif x == 10:
    print("x は 10 と等しい")
else:
    print("x は 10 より小さい")
```

### ループ

```python
for i in range(5):
    print(i)

count = 0
while count < 5:
    print(count)
    count += 1
```

### リスト

```python
fruits = ["りんご", "バナナ", "みかん"]
print(fruits[0])     # りんご
fruits.append("ぶどう")
fruits.remove("バナナ")
print(len(fruits))   # 3
```

### 辞書

```python
person = {"name": "Alice", "age": 30, "city": "Tokyo"}
print(person["name"])        # Alice
person["occupation"] = "Engineer"
del person["city"]
```

### 関数

```python
def add_numbers(a, b):
    return a + b

result = add_numbers(5, 3)
print(result)  # 8
```

複数の返り値：

```python
def multiply_value(a):
    return 100 * a, 1000 * a

hundreds, thousands = multiply_value(5)
print(hundreds, thousands)  # 500 5000
```

### パッケージのインポートと Pandas / GeoPandas

```python
import pandas as pd
import geopandas as gpd

# DataFrame の作成
data = {'Name': ['Alice', 'Bob'], 'Age': [25, 30]}
df = pd.DataFrame(data)
print(df.head())
print(df.describe())

# GeoDataFrame の作成
geo_data = {
    'Name': ['東京', '大阪', '福岡'],
    'Latitude':  [35.6895, 34.6937, 33.5904],
    'Longitude': [139.6917, 135.5023, 130.4017],
}
gdf = gpd.GeoDataFrame(
    geo_data,
    geometry=gpd.points_from_xy(geo_data['Longitude'], geo_data['Latitude'])
)
print(gdf)
```

---

## 6. オブジェクト指向プログラミング（OOP）の基礎

GeoPandas や Cartopy はオブジェクト指向で設計されています。仕組みを理解するとドキュメントの読み方が変わります。

### クラスとインスタンス

```python
class Person:
    def __init__(self, name, age):
        self.name = name  # プロパティ（属性）
        self.age = age

    def greet(self):      # メソッド
        print(f"こんにちは、{self.name}です。{self.age}歳です。")

person1 = Person("Alice", 25)
person1.greet()  # -> こんにちは、Aliceです。25歳です。
```

### 継承

```python
class Student(Person):
    def __init__(self, name, age, student_id):
        super().__init__(name, age)  # 親クラスの __init__ を呼ぶ
        self.student_id = student_id

    def study(self):
        print(f"{self.name} は勉強中です。")

student = Student("Eve", 22, "123456")
student.greet()   # Person から継承したメソッド
student.study()   # Student 独自のメソッド
```

`GeoSeries(GeoPandasBase, Series)` のように、GeoPandas は Pandas の全機能を継承した上で地理情報機能を追加しています。

---

## 7. Matplotlib による可視化

Matplotlib は Python における最も基本的なグラフ描画ライブラリです。Cartopy・GeoPandas など多くの GIS パッケージが内部で使用しています。

### 図の構造

| 要素 | 説明 |
|------|------|
| `Figure` | キャンバス全体 |
| `Axes` | 個々のプロット領域（軸・データを含む） |
| `Axis` | x軸・y軸そのもの（`Axes` と混同注意） |

### 基本プロット

```python
import numpy as np
from matplotlib import pyplot as plt

x = np.arange(-np.pi, np.pi, 0.2)
y = np.sin(x)

plt.plot(x, y)
plt.show()
```

### 複数サブプロット

```python
x = np.arange(-np.pi, np.pi, 0.2)

f, axarr = plt.subplots(2, sharex=True)

axarr[0].plot(x, np.sin(x))
axarr[0].set_title('sine')
axarr[0].set_ylabel('sin(x)')

axarr[1].plot(x, np.cos(x))
axarr[1].set_title('cosine')
axarr[1].set_ylabel('cos(x)')
axarr[1].set_xlabel('x')

plt.show()
```

### プロットスタイルのバリエーション

```python
x = [1, 2, 3, 4, 5]
y = [6, 7, 8, 9, 10]

f, ((ax0, ax1), (ax2, ax3)) = plt.subplots(2, 2)

ax0.plot(x, y, 'r--', label='赤い破線')
ax0.legend(loc='lower right')

ax1.scatter(x, y, c=y, cmap='bwr', s=35)  # 散布図

ax2.bar(x, y, color='k')                  # 棒グラフ

ax3.barh(x, y, color='y')                 # 横棒グラフ

plt.show()
```

---

## 8. Cartopy による地図作成

Cartopy は Matplotlib の `Axes` を拡張した `GeoAxes` を提供し、座標参照系（CRS）を扱えるようにします。

```python
import matplotlib.pyplot as plt
from cartopy import crs as ccrs
from cartopy import feature as cfeature

fig = plt.figure(figsize=(11, 8.5))
ax = plt.subplot(1, 1, 1, projection=ccrs.PlateCarree(central_longitude=0))
ax.set_title("世界地図（Plate Carrée 投影）")
ax.coastlines()
ax.add_feature(cfeature.BORDERS, linewidth=0.5, edgecolor='gray')
plt.show()
```

別の投影法（Lambert Azimuthal Equal Area）：

```python
fig = plt.figure(figsize=(11, 8.5))
projLae = ccrs.LambertAzimuthalEqualArea(central_longitude=0.0, central_latitude=0.0)
ax = plt.subplot(1, 1, 1, projection=projLae)
ax.set_title("ランベルト正積方位図法")
ax.coastlines()
ax.add_feature(cfeature.BORDERS, linewidth=0.5, edgecolor='blue')
plt.show()
```

---

## 9. GeoPandas によるベクターデータの描画

```python
import geopandas as gpd
import matplotlib.pyplot as plt
from cartopy import crs as ccrs

gdf = gpd.read_file(
    'https://raw.githubusercontent.com/GeoScripting-WUR/PythonProgramming/master/data/gadm41_NLD_2.json'
)
gdf = gdf.to_crs(28992)  # オランダ座標系（RD New）
crs = ccrs.epsg(28992)

fig = plt.figure(figsize=(12, 12))
ax = plt.subplot(1, 1, 1, projection=crs)
ax.set_title('オランダの市区町村')

gl = ax.gridlines(draw_labels=True, linewidth=1, color='gray', alpha=0.5, linestyle='--')

min_x, min_y, max_x, max_y = gdf.total_bounds
ax.set_extent((min_x, max_x, min_y, max_y), crs=crs)
ax.add_geometries(gdf["geometry"], crs=crs, edgecolor='black', facecolor='white')

plt.show()
```

---

## 10. Rasterio によるラスターデータの描画

```python
import requests, io, zipfile
import rasterio
from rasterio.plot import show
import geopandas as gpd
import matplotlib.pyplot as plt
from cartopy import crs as ccrs

# Landsat 8 画像をダウンロード・展開
url = 'https://github.com/GeoScripting-WUR/VectorRaster/releases/download/tutorial-data/landsat8.zip'
resp = requests.get(url)
zf = zipfile.ZipFile(io.BytesIO(resp.content))
zf.extractall('./')

crs = ccrs.epsg(32631)  # UTM 31N

fig = plt.figure(figsize=(12, 12))
ax = plt.subplot(1, 1, 1, projection=crs)
ax.set_title('Landsat 8 — オランダ')

gdf = gpd.read_file(
    'https://raw.githubusercontent.com/GeoScripting-WUR/PythonProgramming/master/data/gadm41_NLD_2.json'
)
gdf = gdf.to_crs(32631)
gdf.plot(ax=ax, edgecolor='white', color='none')

dataset = rasterio.open('./LC81970242014109LGN00.tif')
show(dataset, ax=ax, cmap='gist_ncar')

plt.show()
```

---

## 参考リンク

### 原典
- [WUR GeoScripting — Python Environments（原典1）](https://geoscripting-wur.github.io/pythonEnvs/)
- [WUR GeoScripting — Tutorial 9: Python Programming（原典2）](https://geoscripting-wur.github.io/PythonProgramming/)

### ツール
- [uv 公式ドキュメント](https://docs.astral.sh/uv/)
- [VS Code 公式ドキュメント（Python）](https://code.visualstudio.com/docs/python/python-tutorial)
- [VS Code キーボードショートカット（Linux）](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
- [VS Code Source Control ガイド](https://code.visualstudio.com/docs/sourcecontrol/overview)

### ライブラリ
- [Matplotlib 公式ドキュメント](https://matplotlib.org/)
- [Cartopy ギャラリー](https://scitools.org.uk/cartopy/docs/latest/gallery/index.html)
- [Project Pythia — Cartopy 入門](https://foundations.projectpythia.org/core/cartopy/cartopy.html)
- [Python Graph Gallery](https://python-graph-gallery.com/matplotlib/)

### 学習リソース
- [Python 公式チュートリアル](https://docs.python.org/3/contents.html)
- [Python スタイルガイド（PEP 8）](https://www.python.org/dev/peps/pep-0008/)
- [Datacamp: Introduction to Python](https://www.datacamp.com/courses/intro-to-python-for-data-science)
- [Stack Overflow — Python タグ](https://stackoverflow.com/questions/tagged/python)
