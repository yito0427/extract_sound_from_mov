# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This project extracts audio from video files using FFmpeg. The repository is currently in initial setup phase with no implementation yet.

**Purpose**: 動画から音声を抽出する (Extract audio from video files)  
**Core Dependency**: FFmpeg

## Development Guidelines

Since this is a new project, consider these common patterns for audio extraction tools:

### If implementing as a Python script:
```bash
# Setup
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install ffmpeg-python  # or moviepy

# Running
python extract_audio.py input.mov output.mp3
```

### If implementing as a Shell script:
```bash
# Direct FFmpeg usage
ffmpeg -i input.mov -vn -acodec copy output.m4a
ffmpeg -i input.mov -vn -acodec mp3 output.mp3
```

### If implementing as a Node.js application:
```bash
# Setup
npm init -y
npm install fluent-ffmpeg

# Running
node extract.js input.mov output.mp3
```

## FFmpeg Common Options

When implementing the audio extraction:

- `-i input.mov` - Input file
- `-vn` - Disable video recording
- `-acodec copy` - Copy audio codec (no re-encoding, fastest)
- `-acodec mp3` - Convert to MP3
- `-acodec aac` - Convert to AAC
- `-ab 192k` - Set audio bitrate
- `-ar 44100` - Set audio sample rate

## Implementation Suggestions

1. **Input validation**: Check if input file exists and is a valid video format
2. **Output format detection**: Auto-detect based on file extension or provide options
3. **Error handling**: Handle FFmpeg not installed, invalid formats, etc.
4. **Progress indication**: FFmpeg provides progress output that can be parsed
5. **Batch processing**: Support multiple files or directory processing

## Testing Approach

When tests are added:
- Test with various video formats (MOV, MP4, AVI, MKV)
- Test different audio codecs (AAC, MP3, FLAC)
- Test error cases (corrupted files, no audio stream)
- Test batch operations if implemented