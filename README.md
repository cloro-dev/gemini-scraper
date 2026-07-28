# Gemini Scraper API — Grounded Google AI Responses, Sources & Citations

[![Google Gemini scraper by cloro](https://github.com/cloro-dev/gemini-scraper/blob/main/gemini-scraper-hero-image.png)](https://cloro.dev/gemini/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

Scrape Google Gemini responses via API. Returns parsed JSON with full text and markdown, **grounded sources with confidence scores**, **citation pills**, and Gemini's internal reasoning steps. Python, cURL, and Node.js examples below.

Built for developers doing AI brand monitoring on Gemini, source-attribution research, competitive intelligence across Google's AI stack, and content generation at scale — without managing CAPTCHAs, rotating proxies, session state, or Google's anti-bot defenses.

## Quick start

1. Get an API key at [cloro.dev](https://cloro.dev/?utm_source=github&utm_medium=readme).
2. Send a request:

   ```bash
   curl -X POST https://api.cloro.dev/v1/monitor/gemini \
     -H "Authorization: Bearer YOUR_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"prompt": "What are the top climate risks for coastal real estate?"}'
   ```

3. Parse the returned JSON — `result.text`, `result.markdown`, `result.citationPills[]`, `result.sources[]`.

Full examples in Python, cURL, and Node.js below.

## How it works

The Gemini scraper handles the rendering, parsing, and delivery of results in your requested format. You provide your prompt, API credentials, and optional parameters as shown below.

### Request sample (Python)

```python
import json
import requests

# API parameters
payload = {
    'prompt': 'Explain the concept of quantum entanglement',
    'country': 'US',
    'include': {
        'markdown': True
    }
}

# Get a response
response = requests.post(
    'https://api.cloro.dev/v1/monitor/gemini',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload
)

# Print response to stdout
print(response.json())

# Save response to a JSON file
with open('response.json', 'w') as file:
    json.dump(response.json(), file, indent=2)
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/gemini \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Explain the concept of quantum entanglement",
    "country": "US",
    "include": {
      "markdown": true
    }
  }'
```

### Request sample (Node.js)

```javascript
const axios = require("axios");

const payload = {
  prompt: "Explain the concept of quantum entanglement",
  country: "US",
  include: {
    markdown: true,
  },
};

axios
  .post("https://api.cloro.dev/v1/monitor/gemini", payload, {
    headers: {
      Authorization: "Bearer YOUR_API_KEY",
      "Content-Type": "application/json",
    },
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

### Request parameters

| Parameter             | Description                                                                | Default value |
| --------------------- | -------------------------------------------------------------------------- | ------------- |
| `prompt`\*            | The prompt or question to send to Gemini (1-10,000 characters)             | –             |
| `country`             | Optional country/region code for localized results (e.g., `US`, `JP`)      | `US`          |
| `state`               | Optional US state code for state-level geo-targeting (e.g., `CA`, `TX`, `NY`). Only valid when `country` is `US`. See [supported codes](https://api.cloro.dev/v1/states?country=US). | –             |
| `include.markdown`    | Include response in Markdown format when set to true                       | `false`       |
| `include.html`        | Include URL to full HTML response when set to true (URL expires after 24h) | `false`       |
| `include.rawResponse` | Include raw streaming response events for debugging                        | `false`       |

- Mandatory parameters
  \*\* Gemini is currently not available in European countries (EEA/EU).

---

### Output samples

The Gemini Scraper API returns a structured JSON object containing Gemini's AI-generated response and metadata.

**Structured JSON output snippet:**

```json
{
  "success": true,
  "result": {
    "text": "Quantum entanglement is a physical phenomenon that occurs when a group of particles are generated, interact, or share spatial proximity in a way such that the quantum state of each particle of the group cannot be described independently of the state of the others...",
    "sources": [
      {
        "position": 1,
        "url": "https://en.wikipedia.org/wiki/Quantum_entanglement",
        "label": "Wikipedia",
        "description": "Quantum entanglement is a physical phenomenon..."
      }
    ],
    "html": "https://storage.cloro.dev/results/c45a5081-808d-4ed3-9c86-e4baf16c8ab8/page-1.html", // URL expires after 24 hours
    "markdown": "**Quantum entanglement** is a physical phenomenon..."
  }
}
```

### Sources array structure

Gemini responses include a `sources` array with citation details:

| Field         | Type    | Description                                   |
| ------------- | ------- | --------------------------------------------- |
| `position`    | integer | Position order of the source in the response  |
| `url`         | string  | Direct URL to the source content              |
| `label`       | string  | Source name or publication                    |
| `description` | string  | Brief description of what the source contains |

### Citation pills array structure

When the Gemini answer carries pill chips (including section-summary chips), the `result.citationPills` array exposes each cited source as a self-contained entry. When a chip cites N sources, the array contains N entries sharing the same `citationPillId` but with per-source `label`, `url`, and `domain`. Group by `citationPillId` to recover pill-level structure. The field is omitted when no pills are present.

| Field            | Type    | Description                                                                                                                                                       |
| ---------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `label`          | string  | Per-source title from the sources rail (e.g. `"Quantum computing — Wikipedia"`). Always present; may be an empty string when the rail has no title for this source — read `domain` / `url` for source identity in that case. |
| `citationPillId` | integer | 1-based ordinal shared by all entries from the same chip.                                                                                                         |
| `url`            | string  | Direct URL of the cited source.                                                                                                                                   |
| `domain`         | string  | Host extracted from `url`, for grouping and display.                                                                                                              |
| `description`    | string  | Source snippet when present. Omitted when absent.                                                                                                                 |
| `position`       | integer | 1-based position of this source in the sibling `result.sources` array.                                                                                            |

## Practical Gemini scraper use cases

1. **Reasoning:** Solve logic puzzles, math problems, and analytical tasks.
2. **Content creation:** Generate blog posts, marketing copy, and creative stories.
3. **Code generation:** Write, debug, and explain code snippets in various languages.
4. **Data analysis:** Analyze and summarize datasets or documents.
5. **Educational tutoring:** Create learning materials and explanations.
6. **Research assistance:** Synthesize information from multiple cited sources.

## Why choose cloro?

- **Simple integration:** Clean API design with documentation and examples.
- **Reliable performance:** >99% uptime and low latencies.
- **No infrastructure hassle:** We handle rate limiting and browser management.
- **Gemini access:** Access to Google's Gemini models.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping Gemini allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's Gemini scraper unique?

cloro's Gemini endpoint provides access to Google's Gemini AI with:

- **Structured source citations** with URLs, labels, and descriptions
- **Structured data extraction** for direct integration
- **Low-latency infrastructure** with high availability

### What's the recommended timeout for requests?

We don't recommend putting any timeout, given that our system retries automatically. We recommend setting up a retry mechanism in case of failure.

### Does the API support different countries?

Yes, you can specify country codes. Note that Gemini is currently unavailable in European countries (EEA/EU).

### What kind of questions work best with Gemini?

Gemini excels at reasoning, coding, creative writing, and complex analytical tasks.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [cloro.dev/docs](https://cloro.dev/docs/)
- **Gemini scraper page:** [cloro.dev/gemini](https://cloro.dev/gemini/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for comprehensive search result analysis and AI-curated insights.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with advanced features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot for development tools, Microsoft ecosystem research, and enterprise-focused queries.
- **[Gemini](https://cloro.dev/gemini/)** - Extracts structured data from Google Gemini for complex reasoning, content generation, and cited research.
- **[Google Search](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Google News](https://cloro.dev/google-news/)** - Extracts structured news articles from Google News with titles, snippets, sources, dates, and thumbnail images for news monitoring and media tracking.
- **[Grok](https://cloro.dev/grok/)** - Extracts structured data from Grok for current events, news tracking, and real-time information gathering.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts comprehensive structured data from Perplexity AI with real-time web sources, automatically detecting and extracting rich data objects.

## Contact us

If you have questions or need support, join our community at [r/cloroapi](https://www.reddit.com/r/cloroapi/).

---

Built with ❤️ by the cloro team
