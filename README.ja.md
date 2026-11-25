[English](README.md) | **日本語**

# HAND DODGE+ 🎮
### Python × MediaPipe × OpenCV

**Webカメラに映した手の動きだけで操作できる、Python製の障害物回避ゲームです。**

キーボードやマウスを使わず、手を左右に動かすことでキャラクターを操作します。

上から落ちてくる障害物を避けながら、ハイスコアを目指しましょう！

## 🎮 ゲームの特徴

- **ジェスチャー操作**：Webカメラで手の位置を認識してキャラクターを操作
- **スコアシステム**：生存時間や障害物の回避で得点を獲得
- **コンボ機能**：連続回避でスコア倍率が最大4倍に
- **難易度選択**：EASY・NORMAL・HARDの3段階
- **レベルアップ**：時間経過で障害物の速度や出現頻度が上昇
- **アイテム**：ボーナス得点の星、衝突を防ぐシールド
- **ハイスコア保存**：難易度別の最高得点をローカルに保存

## 🛠 使用技術

| 技術 | 用途 |
|---|---|
| Python | ゲーム全体の実装 |
| OpenCV | Webカメラの映像取得・画面描画 |
| MediaPipe | 手の特徴点の検出 |
| NumPy | 画像・座標データの処理 |
| JSON | ハイスコアの保存 |

MediaPipeのHand Landmarkerを使用し、手の21個の特徴点を検出します。

手首や指の付け根の座標から手の中心付近を計算し、ゲーム内のキャラクターの位置に反映しています。

## 💻 動作環境

開発・動作確認環境：

- OS：Windows
- Python：3.13.5
- Webカメラ：PC内蔵またはUSB接続
- MediaPipe：1.2.2
- OpenCV：5.0.0.93
- NumPy：2.5.4

※ほかの環境での動作は未検証です。

## 🚀 インストール方法

### 1. プロジェクトをダウンロード

GitHubの「Code」→「Download ZIP」からダウンロードして展開するか、Gitでリポジトリをクローンしてください。

### 2. 仮想環境を作成

WindowsのPowerShellで、`webcam-game` フォルダに移動してから実行します。

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. 必要なライブラリをインストール

```powershell
python -m pip install -r requirements.txt
```

### 4. 手認識モデルをダウンロード

```powershell
python download_model.py
```

MediaPipe公式の学習済みモデル `hand_landmarker.task` をダウンロードします。

### 5. ゲームを起動

```powershell
python main.py --camera 0 --difficulty normal
```

これでWebカメラが起動し、ゲームを開始できます。

## 🕹 操作方法

| 操作 | 動作 |
|---|---|
| 手を左に動かす | キャラクターが左へ移動 |
| 手を右に動かす | キャラクターが右へ移動 |
| 手を中央で約1.5秒維持 | ゲームオーバー後の再スタート |
| Q / Esc | ゲーム終了 |

カメラが認識されない場合は、`--camera 1` に変更してください。

## ⚙ 難易度設定

起動時に難易度を指定できます。

```powershell
python main.py --difficulty easy
python main.py --difficulty normal
python main.py --difficulty hard
```

| 難易度 | ライフ | レベルアップ間隔 |
|---|---:|---:|
| EASY | 5 | 25秒 |
| NORMAL | 3 | 20秒 |
| HARD | 2 | 16秒 |

レベルが上がるほど障害物が速くなり、回避が難しくなります。

## 🏆 スコアシステム

障害物を回避すると得点を獲得できます。

連続で障害物を回避することでコンボ倍率が増加します。

| コンボ数 | スコア倍率 |
|---|---|
| 0〜3 | 1倍 |
| 4〜7 | 2倍 |
| 8〜11 | 3倍 |
| 12以上 | 4倍 |

障害物回避時の基本的な得点計算：

```python
multiplier = min(4, 1 + combo // 4)
points = 10 * level * multiplier

score += points
```

## 📁 ファイル構成

```text
webcam-game/
├── main.py
├── game_logic.py
├── download_model.py
├── requirements.txt
└── models/
    └── hand_landmarker.task
```

- **main.py**：カメラ入力・手の検出・画面描画
- **game_logic.py**：スコア・障害物・当たり判定・難易度
- **download_model.py**：学習済みモデルのダウンロード
- **requirements.txt**：依存ライブラリ
- **models/**：ダウンロードした手認識モデルの保存先

※モデルファイルはインストール手順で取得します。仮想環境や個人のカメラ映像はGitHubに含めないでください。

## 🔍 仕組み

ゲームは主に以下の流れで処理します。

1. OpenCVでWebカメラの映像を取得
2. MediaPipeで手の21個の特徴点を検出
3. 手の中心付近のX座標を計算
4. 座標をゲーム画面の横位置に変換
5. キャラクター・障害物を更新
6. 当たり判定とスコア計算を実行
7. ゲーム画面を描画

手の位置には平滑化処理を適用し、細かな動きによるキャラクターの揺れを抑えています。

## 🔧 トラブルシューティング

**カメラが起動しない場合**

Windowsの設定でカメラへのアクセスを許可し、ほかのアプリがカメラを使用していないか確認してください。

カメラ番号を変更する方法もあります。

```powershell
python main.py --camera 1
```

**モデルファイルが読み込めない場合**

`download_model.py` を再実行して、モデルファイルが正常にダウンロードされたか確認してください。

**ライブラリの読み込みエラーが出る場合**

仮想環境を有効にしてから、`requirements.txt` のライブラリを再インストールしてください。

## 📚 参考資料

- [MediaPipe Hand Landmarker](https://developers.google.com/edge/mediapipe/solutions/vision/hand_landmarker/python)
- [OpenCV](https://opencv.org/)
- [Python](https://www.python.org/)

---

**RiG++ Advent Calendar 2026 — 12月17日投稿予定**

Pythonを使った画像認識とゲーム開発のサンプルプロジェクトです。
