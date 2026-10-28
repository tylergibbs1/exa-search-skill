# Exa Search Skill for Clawdbot

A [Clawdbot](https://github.com/clawdbot/clawdbot) skill for searching people, companies, and specific content types using [Exa's](https://exa.ai) neural/embeddings-based search API. Useful for building prospect lists, competitive research, and finding specific people or companies.

## Quick start

```bash
cd ~/clawd/skills  # or wherever your skills directory is
git clone https://github.com/tylergibbs1/exa-search-skill.git exa-search
export EXA_API_KEY="your_api_key_here"  # https://dashboard.exa.ai/api-keys
```

Then restart Clawdbot or reload skills. Not using Clawdbot? Grab the curl examples from [examples.md](examples.md) and call the API directly.

## How it works

The skill ([SKILL.md](SKILL.md)) teaches the agent when to reach for Exa instead of general web search, and how to call Exa's API. Exa uses neural embeddings to find content types that traditional search engines struggle with, and supports category filtering and semantic search.

## Features

- **People**: LinkedIn profiles, personal sites
- **Companies**: company pages and profiles, with AI-generated summaries
- **Research papers**: academic publications
- **Tweets, GitHub repos, PDFs**: specific content types
- **Structured extraction**: pull names, titles, and companies out of results

## Example: building a prospect list

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

Returns 50 LinkedIn profiles with names, titles, companies. More in [examples.md](examples.md).

## Installation

1. Clone this skill to your Clawdbot skills directory:
   ```bash
   cd ~/clawd/skills  # or wherever your skills directory is
   git clone https://github.com/tylergibbs1/exa-search-skill.git exa-search
   ```
2. Get your Exa API key: https://dashboard.exa.ai/api-keys
3. Add to your environment (and to `~/.zshrc` or `~/.bashrc` to persist):
   ```bash
   export EXA_API_KEY="your_api_key_here"
   ```
4. Restart Clawdbot or reload skills.

## Use cases for Verity

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

## License

[MIT](LICENSE)
