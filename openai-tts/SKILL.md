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
    "model": "gpt-4o-mini-tts",
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
| `ash` | Clear, direct |
| `coral` | Warm, approachable |
| `echo` | Warm, conversational |
| `fable` | British, storytelling |
| `onyx` | Deep, authoritative |
| `nova` | Friendly, upbeat |
| `sage` | Calm, thoughtful |
| `shimmer` | Soft, gentle |

## Models

| Model | Description | Cost |
|-------|-------------|------|
| `gpt-4o-mini-tts` | Latest, best quality | $0.60/1M input chars |
| `tts-1` | Standard (legacy) | $15/1M chars |
| `tts-1-hd` | High quality (legacy) | $30/1M chars |

**Recommended:** Use `gpt-4o-mini-tts` — better quality and 25x cheaper than legacy models.

## Parameters

```json
{
  "model": "gpt-4o-mini-tts",
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
    "model": "gpt-4o-mini-tts",
    "input": "Hei! Dette er en stemmenote på norsk.",
    "voice": "nova"
  }' \
  --output /tmp/norwegian.mp3
```

### With slower speed
```bash
curl -s https://api.openai.com/v1/audio/speech \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4o-mini-tts",
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
    "model": "gpt-4o-mini-tts",
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
- Use `opus` format for smaller file sizes
- Always use `gpt-4o-mini-tts` (best quality, lowest cost)
