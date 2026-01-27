# Exa Search Examples

Quick reference for common use cases with Exa.

## Setup

```bash
export EXA_API_KEY="your_api_key_here"
```

## 1. Find Medical Billing Managers (Prospect List)

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "medical billing manager at hospital",
    "category": "people",
    "numResults": 100,
    "contents": {
      "text": {"maxCharacters": 500}
    }
  }' | jq '.results[] | {name: .title, url: .url, preview: .text[:200]}'
```

**Output:** 100 LinkedIn profiles with names, titles, companies

**Next steps:**
- Parse the results to extract names and emails
- Save to CSV for cold outreach
- Use Claude Code to process results into structured data

## 2. Find RCM Software Companies (Competitor Analysis)

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
        "query": "What does this company do, who are their customers, and what makes them unique?"
      }
    }
  }' | jq '.results[] | {company: .title, url: .url, summary: .summary}'
```

**Output:** 50 companies with AI-generated summaries

## 3. Find Prior Auth Software Companies

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "prior authorization automation software",
    "type": "deep",
    "additionalQueries": [
      "pre-authorization platform",
      "prior auth workflow software"
    ],
    "category": "company",
    "numResults": 30,
    "contents": {
      "summary": true
    }
  }' | jq -r '.results[] | "\(.title)\n\(.url)\n\(.summary)\n---"'
```

**Output:** Comprehensive list with query expansion

## 4. Find Healthcare Billing Influencers

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "medical billing coding educator consultant",
    "category": "people",
    "numResults": 50,
    "contents": {
      "text": {"maxCharacters": 300}
    }
  }' | jq '.results[] | {name: .title, url: .url}'
```

**Output:** 50 LinkedIn profiles of billing educators/consultants

## 5. Recent Medicare Coverage News

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Medicare coverage policy changes",
    "category": "news",
    "startPublishedDate": "2025-01-01T00:00:00.000Z",
    "numResults": 20,
    "contents": {
      "highlights": {
        "numSentences": 2,
        "highlightsPerUrl": 2
      }
    }
  }' | jq -r '.results[] | "\(.title)\n\(.url)\nHighlights: \(.highlights | join(" | "))\n---"'
```

**Output:** Recent news with key highlights

## 6. Find Hospitals Using Epic EMR

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "hospital using Epic EHR electronic health records",
    "category": "company",
    "numResults": 50,
    "contents": {
      "text": {"maxCharacters": 200}
    }
  }' | jq '.results[] | {hospital: .title, url: .url, text: .text[:150]}'
```

## 7. Competitor Blog Posts (Last 3 Months)

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "prior authorization best practices guide",
    "includeDomains": ["availity.com", "waystar.com", "experian.com"],
    "startPublishedDate": "2024-10-27T00:00:00.000Z",
    "numResults": 20,
    "contents": {
      "summary": true
    }
  }' | jq -r '.results[] | "\(.title)\n\(.url)\nPublished: \(.publishedDate)\n\(.summary)\n---"'
```

## 8. Structured Data Extraction (Funding Announcements)

```bash
curl -X POST https://api.exa.ai/search \
  -H "x-api-key: $EXA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "query": "healthcare technology startup funding announcement",
    "category": "news",
    "startPublishedDate": "2024-11-01T00:00:00.000Z",
    "numResults": 20,
    "contents": {
      "summary": {
        "schema": {
          "type": "object",
          "properties": {
            "companyName": {"type": "string"},
            "fundingAmount": {"type": "string"},
            "fundingRound": {"type": "string"},
            "leadInvestor": {"type": "string"},
            "date": {"type": "string"}
          },
          "required": ["companyName"]
        }
      }
    }
  }' | jq '.results[] | {company: .summary.companyName, amount: .summary.fundingAmount, round: .summary.fundingRound, investor: .summary.leadInvestor, url: .url}'
```

**Output:** Structured JSON for each funding announcement

## Processing Results with jq

### Extract just URLs for further processing

```bash
curl ... | jq -r '.results[].url' > urls.txt
```

### Create CSV of people

```bash
curl ... | jq -r '.results[] | [.title, .url] | @csv' > prospects.csv
```

### Count results by domain

```bash
curl ... | jq -r '.results[].url' | sed 's|https\?://\([^/]*\).*|\1|' | sort | uniq -c
```

## Integration with Verity Marketing

### Building Email Lists

1. Run people search → get LinkedIn URLs
2. Use LinkedIn profile scrapers (outside Exa) to get emails
3. Feed into cold email tool

### Content Gap Analysis

1. Search competitor domains for content
2. Identify topics they're covering
3. Create better content on those topics

### Partner Discovery

1. Search for companies in adjacent spaces
2. Extract summaries and contact info
3. Reach out for partnerships

### Influencer Outreach

1. Find medical billing educators/consultants
2. Get their LinkedIn/personal sites
3. Send them Verity for review/testimonial

## Tips

- **Start with 10-20 results** to test queries, then scale to 50-100
- **Use `jq` to process JSON** - it's way easier than parsing manually
- **Save results to files** for later processing
- **Combine with Claude Code** for advanced data extraction/processing
- **Use `deep` search** when you want comprehensive coverage
- **Category filters are powerful** - always specify when you know what you want
