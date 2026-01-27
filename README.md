# Exa Search Skill

Search for people, companies, and specific content types using Exa's neural/embeddings-based search API.

## What is Exa?

Exa is a search API that uses neural embeddings to find specific types of content that traditional search engines struggle with. It's especially good at finding:

- **People** - LinkedIn profiles, personal sites
- **Companies** - Company pages and profiles
- **Research papers** - Academic publications
- **Tweets, GitHub repos, PDFs** - Specific content types

## Why Use This?

Tyler wanted to build prospect lists for Verity (medical billing managers, RCM companies, etc.). Exa is **perfect** for this:

1. **Find 100 medical billing managers** with LinkedIn profiles
2. **Find RCM software companies** with summaries
3. **Find recent news** about competitors
4. **Extract structured data** from results (names, titles, companies)

Traditional search (Brave) can't do category filtering or semantic search like this.

## Getting Started

1. Get API key: https://dashboard.exa.ai/api-keys
2. Export it: `export EXA_API_KEY="your_key"`
3. Read SKILL.md for usage examples

## Example: Building a Prospect List

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "medical billing manager at hospital in Texas",
    "category": "people",
    "numResults": 50,
    "contents": {
      "text": {"maxCharacters": 500}
    }
  }'
```

Returns 50 LinkedIn profiles with names, titles, companies.

## Use Cases for Verity

- Find medical billing managers to cold email
- Find RCM software companies (competitors/partners)
- Find prior auth software companies
- Find healthcare billing influencers
- Research competitor blog posts/content
- Find hospitals using specific EMR systems

## Cost

- Search: $0.005 for 1-25 results (neural)
- Deep search: $0.015 for 1-25 results
- Content extraction: $0.001 per page

Very affordable for building lists.
