# extract_sound_from_mov

動画ファイルから音声を抽出するツール。FFmpegを利用して高品質な音声抽出を実現します。

## 概要

このツールは、MOVファイルなどの動画から音声トラックを抽出し、M4A形式で保存します。複数ファイルの一括処理に対応しており、会議録画や講演動画から音声のみを取り出したい場合に便利です。

## 必要な環境

- FFmpeg（インストールされていること）
- macOS / Linux / Windows

### FFmpegのインストール方法

**macOS (Homebrew使用):**
```bash
brew install ffmpeg
```

**Ubuntu/Debian:**
```bash
sudo apt update
sudo apt install ffmpeg
```

**Windows:**
[FFmpeg公式サイト](https://ffmpeg.org/download.html)からダウンロードし、パスを通してください。

## 使い方

### 1. 基本的な使用方法

```bash
# 動画ファイルを movie/ ディレクトリに配置
mkdir -p movie
cp your_video.mov movie/

# 音声を抽出
ffmpeg -i "movie/your_video.mov" -vn -acodec aac -ab 192k "output/your_video.m4a"
```

### 2. 複数ファイルの一括処理

```bash
# output ディレクトリを作成
mkdir -p output

# movie/ 内のすべての.MOVファイルから音声を抽出
for file in movie/*.MOV; do
    filename=$(basename "$file" .MOV)
    ffmpeg -i "$file" -vn -acodec aac -ab 192k "output/${filename}.m4a"
done
```

### 3. シェルスクリプトでの自動化

`extract_audio.sh`として保存:

```bash
#!/bin/bash

# ディレクトリの準備
mkdir -p output

# カウンター初期化
count=0
total=$(ls movie/*.MOV 2>/dev/null | wc -l)

if [ $total -eq 0 ]; then
    echo "movie/ ディレクトリに.MOVファイルが見つかりません"
    exit 1
fi

echo "合計 $total ファイルを処理します..."

# 各ファイルを処理
for file in movie/*.MOV; do
    if [ -f "$file" ]; then
        count=$((count + 1))
        filename=$(basename "$file" .MOV)
        echo "[$count/$total] 処理中: $filename"
        
        ffmpeg -i "$file" -vn -acodec aac -ab 192k "output/${filename}.m4a" -y -loglevel error
        
        if [ $? -eq 0 ]; then
            echo "✓ 完了: ${filename}.m4a"
        else
            echo "✗ エラー: $filename の処理に失敗しました"
        fi
    fi
done

echo "すべての処理が完了しました！"
```

実行方法:
```bash
chmod +x extract_audio.sh
./extract_audio.sh
```

## FFmpegオプションの説明

- `-i` : 入力ファイルを指定
- `-vn` : 動画を無効化（音声のみ抽出）
- `-acodec aac` : 音声コーデックをAACに指定
- `-ab 192k` : 音声ビットレートを192kbpsに設定

## その他の音声形式への変換

### MP3形式で出力
```bash
ffmpeg -i "movie/video.mov" -vn -acodec mp3 -ab 192k "output/audio.mp3"
```

### WAV形式（無圧縮）で出力
```bash
ffmpeg -i "movie/video.mov" -vn -acodec pcm_s16le "output/audio.wav"
```

### 音声をそのままコピー（再エンコードなし）
```bash
ffmpeg -i "movie/video.mov" -vn -acodec copy "output/audio.m4a"
```

## ディレクトリ構造

```
extract_sound_from_mov/
├── README.md
├── LICENSE
├── CLAUDE.md
├── .gitignore
├── movie/          # 入力動画ファイルを配置（Gitで追跡されません）
│   ├── video1.MOV
│   ├── video2.MOV
│   └── ...
└── output/         # 抽出された音声ファイル（Gitで追跡されません）
    ├── video1.m4a
    ├── video2.m4a
    └── ...
```

## 注意事項

- `movie/` と `output/` ディレクトリは `.gitignore` に含まれているため、GitHubにはアップロードされません
- 大きなファイルの処理には時間がかかる場合があります
- 十分なディスク容量があることを確認してください

## ライセンス

MIT License - 詳細は [LICENSE](LICENSE) ファイルを参照してください

## 作者

Yoshiyuki Ito

Copyright (c) 2025