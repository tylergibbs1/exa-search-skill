---
name: exa-search
description: Search for people (LinkedIn profiles), companies, research papers, tweets, GitHub repos, and other specific content types using Exa's neural/embeddings-based search API. Use when you need to find specific people, companies, or content categories beyond general web search. Especially useful for finding LinkedIn profiles, company pages, recent research, or building prospect lists.
---

# Exa Search

Exa is a neural search API optimized for finding **specific types of content** - especially people, companies, and structured information that traditional search struggles with.

## When to Use Exa vs. web_search

**Use Exa when:**
- Finding people (LinkedIn profiles, personal sites)
- Finding companies (company pages, profiles)
- Searching within specific content types (research papers, PDFs, tweets, GitHub repos)
- Building prospect/contact lists
- Need semantic/embeddings-based search
- Want structured extraction (summaries with JSON schema)

**Use web_search (Brave) when:**
- General web search
- Recent news/events
- Simple lookups

## Setup

Get API key from: https://dashboard.exa.ai/api-keys

Store in environment:
```bash
export EXA_API_KEY="your_api_key_here"
```

## Core Features

### 1. Category Search

Search within specific content types using `category`:

- **`people`** - LinkedIn profiles (high quality)
- **`company`** - Company pages (high quality)
- **`research paper`** - Academic papers
- **`news`** - News articles
- **`pdf`** - PDF documents
- **`github`** - GitHub repositories
- **`tweet`** - Twitter/X posts
- **`personal site`** - Personal websites/blogs
- **`financial report`** - Financial documents

**Example - Find people:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "medical billing managers at hospitals in Texas",
    "category": "people",
    "numResults": 20
  }'
```

**Example - Find companies:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "revenue cycle management software companies",
    "category": "company",
    "numResults": 10
  }'
```

### 2. Search Types

- **`auto`** (default) - Intelligently picks best method
- **`neural`** - Embeddings-based semantic search
- **`deep`** - Query expansion + comprehensive results (supports `additionalQueries`)
- **`fast`** - Streamlined/faster version

**Example - Deep search with query variations:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "prior authorization software",
    "type": "deep",
    "additionalQueries": [
      "pre-authorization automation",
      "prior auth platforms"
    ],
    "numResults": 20
  }'
```

### 3. Content Extraction

Get full text, highlights, or summaries:

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "latest LLM benchmarks",
    "category": "research paper",
    "numResults": 5,
    "contents": {
      "text": {
        "maxCharacters": 2000
      },
      "highlights": {
        "numSentences": 3,
        "highlightsPerUrl": 2
      },
      "summary": {
        "query": "key findings and results"
      }
    }
  }'
```

**Structured summaries with JSON schema:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Series B funding announcements healthcare",
    "category": "news",
    "numResults": 10,
    "contents": {
      "summary": {
        "schema": {
          "type": "object",
          "properties": {
            "companyName": {"type": "string"},
            "fundingAmount": {"type": "string"},
            "leadInvestor": {"type": "string"},
            "date": {"type": "string"}
          },
          "required": ["companyName", "fundingAmount"]
        }
      }
    }
  }'
```

### 4. Date Filtering

Filter by published date or crawl date:

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Medicare coverage changes",
    "category": "news",
    "startPublishedDate": "2025-01-01T00:00:00.000Z",
    "numResults": 10
  }'
```

### 5. Domain Filtering

Include or exclude specific domains:

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "machine learning papers",
    "includeDomains": ["arxiv.org", "openreview.net"],
    "numResults": 20
  }'
```

## Common Use Cases

### Building Prospect Lists

**Find 100 medical billing managers:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "medical billing manager at hospital",
    "category": "people",
    "numResults": 100,
    "contents": {
      "text": {
        "maxCharacters": 500
      }
    }
  }'
```

Process results to extract names, titles, companies, LinkedIn URLs.

### Finding Companies in a Space

**Find RCM software companies:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "revenue cycle management software company",
    "category": "company",
    "numResults": 50,
    "contents": {
      "summary": {
        "query": "What does this company do and who are their customers?"
      }
    }
  }'
```

### Research/Competitive Analysis

**Find recent competitor blog posts:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "prior authorization automation",
    "includeDomains": ["competitor1.com", "competitor2.com"],
    "startPublishedDate": "2024-11-01T00:00:00.000Z",
    "numResults": 20,
    "contents": {
      "text": true
    }
  }'
```

### Finding Recent Papers/News

**Latest AI research on specific topic:**
```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "LLM agent frameworks",
    "category": "research paper",
    "startPublishedDate": "2025-01-01T00:00:00.000Z",
    "numResults": 10,
    "contents": {
      "highlights": {
        "numSentences": 2,
        "highlightsPerUrl": 3
      }
    }
  }'
```

## Important Constraints

### Category-Specific Limitations

**For `people` and `company` categories**, these filters are NOT supported:
- `startPublishedDate` / `endPublishedDate`
- `startCrawlDate` / `endCrawlDate`
- `includeText` / `excludeText`
- `excludeDomains`

For `people` category:
- `includeDomains` only accepts LinkedIn domains

Using unsupported parameters returns 400 error.

### Result Limits

- **`neural`** / **`auto`**: max 100 results
- **`deep`**: max 100 results
- Contact sales for higher limits

## Tips

1. **Be specific in queries** - "medical billing manager at hospital in Texas" beats "billing manager"
2. **Use `deep` search** for comprehensive results when you need variations
3. **Combine with date filters** for recent content
4. **Request structured summaries** when building lists/databases
5. **Use highlights over full text** for cost efficiency when you need snippets
6. **Category search is powerful** - use it! Much better results than generic search

## Cost-Efficient Patterns

- Use `highlights` instead of full `text` when possible (cheaper)
- Set `maxCharacters` limits on text extraction
- Use `numResults` wisely - start small, increase if needed
- `auto` search type usually best - only use `deep` when necessary

## Environment Variable

```bash
# Add to your shell profile or .env
export EXA_API_KEY="your_api_key_here"
```

## More Info

- Dashboard: https://dashboard.exa.ai
- API Docs: https://exa.ai/docs
- Get API Key: https://dashboard.exa.ai/api-keys
