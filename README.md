[English](README.md) | [日本語](README.ja.md)

# HAND DODGE+ — Webcam Hand-Control Game

An obstacle-dodging game you play with nothing but your hand in front of a webcam — no keyboard or mouse. Built with Python, OpenCV and MediaPipe.

![Gameplay preview](RiG_Webcam_Game/webcam-game/examples/game-preview.png)

## Problem

Show that real-time computer vision can be a practical game controller using only a normal webcam, and keep the game logic cleanly separated and testable.

## How it works

1. OpenCV captures webcam frames.
2. MediaPipe Hand Landmarker detects 21 hand landmarks.
3. The hand's centre X-coordinate is computed, smoothed to suppress jitter, and mapped to the character's horizontal position.
4. Game logic updates obstacles, collisions, score and difficulty; the frame is rendered.

## Features

- Gesture control: move your hand left/right to move the character
- Three difficulties: EASY / NORMAL / HARD (different lives and level-up intervals)
- Combo scoring with up to 4× multiplier; level-ups speed up obstacles
- Power-ups: bonus stars and collision-blocking shields
- Local high scores per difficulty
- Game logic isolated in `game_logic.py` with unit tests

## Tech stack

Python · OpenCV · MediaPipe (Hand Landmarker) · NumPy

## Setup (Windows, PowerShell)

```powershell
cd RiG_Webcam_Game\webcam-game
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python download_model.py          # fetches MediaPipe's hand_landmarker.task
python main.py --camera 0 --difficulty normal
```

If the camera is not found, try `--camera 1`. Developed and tested on Windows with Python 3.13; other platforms are untested. Camera frames are processed locally and not sent anywhere.

## Notes

The Japanese README has full details (controls, scoring table, troubleshooting): [README.ja.md](README.ja.md). The bundled hand-landmark model is Google's MediaPipe model and is subject to its own license.
