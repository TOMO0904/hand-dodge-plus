[English](README.md) | **日本語**

# HAND DODGE+ — Webカメラで手を使って操作するゲーム

Webカメラの前で手を動かすだけで遊べる、障害物よけゲームです。キーボードもマウスも使いません。Python、OpenCV、MediaPipeで作りました。

![プレイ画面](RiG_Webcam_Game/webcam-game/examples/game-preview.png)

## 課題

普通のWebカメラだけで、リアルタイムの画像認識が実用的なゲームコントローラーになることを示すこと。あわせて、ゲームロジックをきれいに分離してテストできるようにすることを目指しました。

## 仕組み

1. OpenCVでWebカメラの映像を取得します。
2. MediaPipe Hand Landmarkerで手の21個のランドマークを検出します。
3. 手の中心のX座標を求め、ぶれを抑えるようになめらかにしてから、キャラクターの横位置に対応させます。
4. ゲームロジックが障害物・当たり判定・スコア・難易度を更新し、画面を描画します。

## 主な機能

- ジェスチャー操作: 手を左右に動かすとキャラクターが動く
- 3つの難易度: EASY / NORMAL / HARD（ライフ数とレベルアップ間隔が違う）
- 最大4倍のコンボ倍率。レベルが上がると障害物が速くなる
- パワーアップ: ボーナスの星と、衝突を防ぐシールド
- 難易度ごとのローカルハイスコア
- ゲームロジックを `game_logic.py` に分離し、ユニットテストを用意

## 技術スタック

Python · OpenCV · MediaPipe（Hand Landmarker） · NumPy

## セットアップ（Windows、PowerShell）

```powershell
cd RiG_Webcam_Game\webcam-game
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python download_model.py          # MediaPipe の hand_landmarker.task を取得
python main.py --camera 0 --difficulty normal
```

カメラが見つからない場合は `--camera 1` を試してください。WindowsとPython 3.13で開発・確認しています。他の環境では未確認です。カメラの映像はPC内だけで処理し、外部には送信しません。

## 補足

操作方法、スコア表、トラブルシューティングなどの詳しい説明は [docs/DETAILS.ja.md](docs/DETAILS.ja.md) にあります。同梱の手のランドマークモデルはGoogleのMediaPipeのモデルで、そのライセンスに従います。
