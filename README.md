# Google Gemini Scraper

[![Google Gemini scraper by cloro](https://github.com/cloro-dev/gemini-scraper/blob/main/gemini-scraper-hero-image.png)](https://cloro.dev/gemini/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google Gemini Scraper](https://cloro.dev/gemini/) by cloro enables developers to programmatically interact with Google's Gemini AI and automatically collect AI-powered responses along with structured metadata. Instead of manual data collection, you can retrieve results as parsed JSON, raw HTML, or other formats for seamless integration into your workflows.

You can use cloro's Gemini Scraper for complex reasoning tasks, content generation, and general knowledge queries. It handles dynamic AI-generated content, supports real-time extraction, and eliminates the need to manage authentication, sessions, or anti-bot systems.

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
  -d
    "{
    "prompt": "Explain the concept of quantum entanglement",
    "country": "US",
    "include": {
      "markdown": true
    }
  }"
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

| Parameter          | Description                                                                | Default value |
| ------------------ | -------------------------------------------------------------------------- | ------------- |
| `prompt`\*         | The prompt or question to send to Gemini (1-10,000 characters)             | –             |
| `country`          | Optional country/region code for localized results (e.g., `US`, `JP`)      | `US`          |
| `include.markdown` | Include response in Markdown format when set to true                       | `false`       |
| `include.html`     | Include URL to full HTML response when set to true (URL expires after 48h) | `false`       |

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
        "description": "Quantum entanglement is a physical phenomenon...",
        "confidence_level": 95
      }
    ],
    "html": "https://storage.cloro.dev/results/c45a5081-808d-4ed3-9c86-e4baf16c8ab8/page-1.html", // URL expires after 48 hours
    "markdown": "**Quantum entanglement** is a physical phenomenon..."
  }
}
```

### Sources array structure

Gemini responses include a `sources` array with confidence levels:

| Field              | Type    | Description                                   |
| ------------------ | ------- | --------------------------------------------- |
| `position`         | integer | Position order of the source in the response  |
| `url`              | string  | Direct URL to the source content              |
| `label`            | string  | Source name or publication                    |
| `description`      | string  | Brief description of what the source contains |
| `confidence_level` | integer | Confidence score for the source (0-100)       |

## Practical Gemini scraper use cases

1. **Complex reasoning:** Solve logic puzzles, math problems, and complex analytical tasks.
2. **Content creation:** Generate high-quality blog posts, marketing copy, and creative stories.
3. **Code generation:** Write, debug, and explain code snippets in various languages.
4. **Data analysis:** Analyze and summarize complex datasets or documents.
5. **Educational tutoring:** Create personalized learning materials and explanations.
6. **Research assistance:** Synthesize information from multiple sources with confidence scoring.

## Why choose cloro?

- **Simple integration:** Clean API design with comprehensive documentation and examples.
- **Reliable performance:** >99% uptime and low latencies.
- **No infrastructure hassle:** We handle rate limiting and browser management.
- **Advanced AI access:** Easy access to Google's powerful Gemini models.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping Gemini allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's Gemini scraper unique?

cloro's Gemini endpoint provides reliable access to Google's Gemini AI with:

- **Source confidence scoring** to evaluate the reliability of citations
- **Structured data extraction** for seamless integration
- **High-performance infrastructure** ensuring low latency and high availability

### What's the recommended timeout for requests?

We recommend setting a timeout of 30-60 seconds. Our system handles automatic retries, but implementing your own retry logic provides the best reliability.

### Does the API support different countries?

Yes, you can specify country codes. Note that Gemini is currently unavailable in European countries (EEA/EU).

### What kind of questions work best with Gemini?

Gemini excels at reasoning, coding, creative writing, and complex analytical tasks.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [docs.cloro.dev](https://docs.cloro.dev)
- **Gemini scraper page:** [cloro.dev/gemini](https://cloro.dev/gemini/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for comprehensive search result analysis and AI-curated insights.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with advanced features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot for development tools, Microsoft ecosystem research, and enterprise-focused queries.
- **[Google](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts comprehensive structured data from Perplexity AI with real-time web sources, automatically detecting and extracting rich data objects.

## Contact us

If you have questions or need support, reach out to us on [our contact page](https://cloro.dev/contact).

---

Built with ❤️ by the cloro team
