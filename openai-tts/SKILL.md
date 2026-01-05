---
name: openai-tts
description: This skill should be used when generating voice notes or audio responses. Use OpenAI's TTS API to convert text to speech and send audio messages back to users.
---

# OpenAI Text-to-Speech (TTS)

## Overview

Generate voice notes using OpenAI's TTS API. Useful for sending audio responses to users on Telegram, WhatsApp, etc.

## Requirements

- `OPENAI_API_KEY` environment variable set

## Quick Usage

```bash
curl -s https://api.openai.com/v1/audio/speech \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1",
    "input": "Your text here",
    "voice": "onyx"
  }' \
  --output /tmp/voice.mp3
```

Then send with: `MEDIA:/tmp/voice.mp3`

## Available Voices

| Voice | Description |
|-------|-------------|
| `alloy` | Neutral, balanced |
| `echo` | Warm, conversational |
| `fable` | British, storytelling |
| `onyx` | Deep, authoritative |
| `nova` | Friendly, upbeat |
| `shimmer` | Soft, gentle |

## Models

| Model | Quality | Speed | Cost |
|-------|---------|-------|------|
| `tts-1` | Standard | Fast | $0.015/1K chars |
| `tts-1-hd` | High quality | Slower | $0.030/1K chars |

## Parameters

```json
{
  "model": "tts-1",           // or "tts-1-hd"
  "input": "Text to speak",   // Max 4096 characters
  "voice": "onyx",            // See voices above
  "response_format": "mp3",   // mp3, opus, aac, flac, wav, pcm
  "speed": 1.0                // 0.25 to 4.0
}
```

## Examples

### Norwegian voice note
```bash
curl -s https://api.openai.com/v1/audio/speech \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1",
    "input": "Hei! Dette er en stemmenote på norsk.",
    "voice": "nova"
  }' \
  --output /tmp/norwegian.mp3
```

### High quality with slower speed
```bash
curl -s https://api.openai.com/v1/audio/speech \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1-hd",
    "input": "Important announcement...",
    "voice": "onyx",
    "speed": 0.9
  }' \
  --output /tmp/announcement.mp3
```

### Opus format (smaller files)
```bash
curl -s https://api.openai.com/v1/audio/speech \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1",
    "input": "Compact audio message",
    "voice": "alloy",
    "response_format": "opus"
  }' \
  --output /tmp/message.opus
```

## Workflow

1. Generate audio with curl
2. Save to /tmp/voice-{timestamp}.mp3
3. Send with `MEDIA:/tmp/voice-{timestamp}.mp3`

## Best Practices

- Use `onyx` or `echo` for professional/calm tone
- Use `nova` for friendly/upbeat messages
- Keep messages concise (under 1 minute ideal)
- Use `tts-1` for speed, `tts-1-hd` for quality
- Use `opus` format for smaller file sizes
