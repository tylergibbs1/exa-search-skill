# Exa Search Skill for Clawdbot

A [Clawdbot](https://github.com/clawdbot/clawdbot) skill for searching people, companies, and specific content types using [Exa's](https://exa.ai) neural/embeddings-based search API.

## What is Exa?

Exa is a search API that uses neural embeddings to find specific types of content that traditional search engines struggle with. It's especially good at finding:

- **People** - LinkedIn profiles, personal sites
- **Companies** - Company pages and profiles
- **Research papers** - Academic publications
- **Tweets, GitHub repos, PDFs** - Specific content types

## Why Use This?

Perfect for building prospect lists, competitive research, and finding specific people/companies:

1. **Find 100 medical billing managers** with LinkedIn profiles
2. **Find companies in specific industries** with AI-generated summaries
3. **Find recent news** about competitors
4. **Extract structured data** from results (names, titles, companies)

Traditional search engines can't do category filtering or semantic search like this.

## Installation

### For Clawdbot Users

1. Clone this skill to your Clawdbot skills directory:
```bash
cd ~/clawd/skills  # or wherever your skills directory is
git clone https://github.com/Tylerbryy/exa-search-skill.git exa-search
```

2. Get your Exa API key: https://dashboard.exa.ai/api-keys

3. Add to your environment:
```bash
export EXA_API_KEY="your_api_key_here"
# Add to ~/.zshrc or ~/.bashrc to persist
```

4. Restart Clawdbot or reload skills

### For Direct Use (Without Clawdbot)

Just grab the examples from `examples.md` and use curl directly with your API key.

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
