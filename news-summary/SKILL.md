---
name: news-summary
description: This skill should be used when the user asks for news updates, daily briefings, or what's happening in the world. Fetches news from NRK (Norwegian) and BBC (international) RSS feeds and can create voice summaries.
---

# News Summary

## Overview

Fetch and summarize news from trusted sources:
- **NRK** (Norwegian) — state broadcaster, primary source
- **BBC World** (International) — supplement for global news

## RSS Feeds

### NRK (Norwegian)
```bash
# Top stories
curl -s "https://www.nrk.no/toppsaker.rss"

# By category
curl -s "https://www.nrk.no/nyheter/siste.rss"      # Latest
curl -s "https://www.nrk.no/sport/siste.rss"        # Sports
curl -s "https://www.nrk.no/kultur/siste.rss"       # Culture
```

### BBC (International)
```bash
# World news
curl -s "https://feeds.bbci.co.uk/news/world/rss.xml"

# Top stories
curl -s "https://feeds.bbci.co.uk/news/rss.xml"
```

## Parse RSS

Extract titles and descriptions:
```bash
curl -s "https://www.nrk.no/toppsaker.rss" | \
  grep -E "<title>|<description>" | \
  sed 's/<[^>]*>//g' | \
  sed 's/^[ \t]*//' | \
  head -30
```

## Workflow

### Text summary
1. Fetch NRK toppsaker
2. Fetch BBC world headlines
3. Summarize in Norwegian
4. Group by category (Norge, Verden, Vær, Sport)

### Voice summary
1. Create text summary
2. Generate voice with OpenAI TTS
3. Send as audio message

```bash
curl -s https://api.openai.com/v1/audio/speech \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1-hd",
    "input": "<news summary text>",
    "voice": "onyx",
    "speed": 0.95
  }' \
  --output /tmp/news.mp3
```

## Example Output Format

```
📰 Nyhetsoppsummering [dato]

🇳🇴 NORGE
- [headline 1]
- [headline 2]

🌍 VERDEN
- [headline 1]
- [headline 2]

⛅ VÆR
- [relevant weather news]
```

## Best Practices

- Keep summaries concise (5-8 top stories)
- Prioritize breaking news and major events
- For voice: ~2 minutes max
- Use Norwegian for delivery
- Cite source if asked (NRK, BBC)
