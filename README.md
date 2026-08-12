# Gemini Scraper

[![Gemini Scraper by cloro](https://github.com/cloro-dev/gemini-scraper/blob/main/gemini-scraper-hero-image.png)](https://cloro.dev/gemini/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Gemini scraper](https://cloro.dev/gemini/?utm_source=github) by cloro returns Google Gemini answers as structured JSON: the answer text and markdown, and every grounded source with position, label and description.

## How do you scrape Gemini?

1. Get an API key at [cloro.dev](https://cloro.dev/?utm_source=github&utm_medium=readme).
2. POST a prompt to `https://api.cloro.dev/v1/monitor/gemini`.
3. Read the parsed fields from the JSON response.

Gemini runs on a separate authenticated domain from Google Search, so it is a different scraping problem from AI Mode or AI Overview even though all three are Google surfaces. The grounded sources panel is what cloro parses; the answer text alone is available from the model API and is not what this endpoint is for.

### Request sample (Python)

```python
import requests

payload = {
    'prompt': 'how do AI visibility tracking tools work',
    'country': 'US',
    'include': {'markdown': True},
}

response = requests.post(
    'https://api.cloro.dev/v1/monitor/gemini',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload,
)

print(response.json())
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/gemini \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "how do AI visibility tracking tools work", "country": "US"}'
```

Node.js and async/webhook examples are in the [endpoint documentation](https://cloro.dev/docs/api-reference/endpoint/monitor-gemini).

### Request parameters

| Parameter | Description | Default |
| --- | --- | --- |
| `prompt`\* | The query or question (1-10,000 characters) | – |
| `country` | Country code for localized results (`US`, `GB`, `DE`) | `US` |
| `state` | US state code for finer localization | – |
| `include.markdown` | Return the answer as Markdown | `false` |
| `include.html` | Return a URL to the full HTML (expires after 24h) | `false` |
| `include.rawResponse` | Return the unparsed upstream payload | `false` |

\* Required

## What data does the Gemini scraper return?

```json
{
  "success": true,
  "result": {
    "text": "AI visibility tracking tools run scheduled prompts against AI engines and parse...",
    "sources": [
      { "position": 1, "url": "https://example.com/geo-guide", "label": "Example GEO Guide", "description": "How AI visibility tools collect data..." }
    ],
    "markdown": "AI visibility tracking tools run scheduled prompts..."
  }
}
```

Alongside `text` and `markdown`:

1. **`sources`** — the grounded sources panel, with position, label and description per source.
2. **`citationPills`** — inline citation chips where the answer carries them.
3. **`rawResponse`** — the unparsed upstream payload.

Gemini returns a narrower structure than AI Mode or ChatGPT. There are no shopping or place arrays on this surface.

Full field-level schemas are in the [endpoint reference](https://cloro.dev/docs/api-reference/endpoint/monitor-gemini).

## Use cases

- **Brand monitoring on Google's assistant surface**, which is distinct from AI Mode and AI Overview.
- **Grounding research** — which domains Gemini treats as authoritative on a question.
- **Cross-engine comparison** — run the same prompt across Gemini, ChatGPT, Perplexity and Copilot and compare source overlap.
- **Content validation** — check whether Gemini describes your product correctly, not just whether it names you.

## FAQ

### How is this different from the Gemini API?

The model API returns generated text. This returns what the Gemini product shows, including the grounded sources panel, which is the part that matters for visibility work.

### Is Gemini the same as Google AI Mode?

No. AI Mode sits inside Google Search and is reached with `udm=50`; Gemini is a separate product on its own domain, with its own citation behaviour.

### Is scraping Gemini allowed?

cloro reads publicly visible responses. Check your own jurisdiction and terms.

### Does it support geo-targeting?

Yes, via `country` and `state`.

## Learn more

- **Endpoint reference:** [cloro.dev/docs](https://cloro.dev/docs/api-reference/endpoint/monitor-gemini)
- **Product page:** [cloro.dev/gemini](https://cloro.dev/gemini/)

## Other cloro scrapers

[AI Mode](https://cloro.dev/ai-mode/) · [AI Overview](https://cloro.dev/ai-overview/) · [ChatGPT](https://cloro.dev/chatgpt/) · [Copilot](https://cloro.dev/copilot/) · [Google Search](https://cloro.dev/google-search/) · [Google News](https://cloro.dev/google-news/) · [Grok](https://cloro.dev/grok/) · [Perplexity](https://cloro.dev/perplexity/)

## Contact us

Questions or support: [r/cloroapi](https://www.reddit.com/r/cloroapi/).
