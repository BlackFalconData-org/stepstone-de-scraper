# StepStone.de Scraper - Germany Job Listings, Salary & Skills

Extract structured data from [stepstone.de](https://stepstone.de) — Germany's largest job board stepstone.de for structured job data including salary range, skills, and employment type. Incremental mode tracks new and changed jobs across runs via a stable stateKey. Filter by German federal state (bundesland) or a radius around any city.

**[StepStone.de Scraper - Germany Job Listings, Salary & Skills on Apify →](https://apify.com/blackfalcondata/stepstone-de-scraper?fpr=1h3gvi)**

---

## Key features



**Search with filters** — Search by keyword and location. Filter by federal state (bundesland), sort by, radius (km), and more.

**Multiple input modes** — full (all jobs) or incremental (new/changed only). Switch modes without re-scraping.

**Detail enrichment** — Fetch full job descriptions, salary data, employer profiles for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings (up to 10.000). Set to 0 for the full catalog.

**Proxy support** — Route traffic through Apify Proxy or your own proxy group to avoid regional blocks and rate limits.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases



**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from stepstone.de on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from stepstone.de.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on stepstone.de to inform pricing decisions, hiring plans, or candidate negotiations.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "software engineer",
  "maxResults": 50,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job search keywords (e.g. "software engineer"). Use JSON array ["query1", "query2"] for multi-query. |
| `location` | string | — | City or region (e.g. "Berlin", "München"). Use JSON array for multi-location. |
| `startUrls` | array | — | Direct StepStone search or job detail URLs. Overrides query/location when provided. |
| `bundesland` | enum | — | Filter by German federal state. Overrides location when set. |
| `sort` | enum | `"relevance"` | Sort order for results |
| `age` | integer | — | Only show jobs posted within this many days (1, 3, 7, or 14) |
| `remote` | boolean | — | Only show remote/home office positions |
| `radius` | enum | — | Search radius around location in km (requires location) |
| `minSalary` | integer | — | Minimum annual salary filter (EUR) |
| `contractType` | enum | — | Filter by contract/employment type |
| `experience` | enum | — | Filter by experience level |
| `workType` | enum | — | Filter by work schedule (full-time / part-time) |
| `companyId` | integer | — | Filter by StepStone company ID (numeric) |
| `language` | string | — | Filter by job posting language (e.g., 'en', 'de') |
| `applicationMethod` | enum | — | Filter by how to apply |
| `excludeSponsored` | boolean | `false` | Skip sponsored/promoted listings. Filters out items where isSponsored=true. |
| `includeDetails` | boolean | `true` | Fetch each job's detail page for full description, structured salary, company profile, and similar jobs. Slower but much richer data. |
| `descriptionFormat` | enum | `"all"` | Which description formats to include in output. |
| `maxResults` | integer | `50` | Maximum total job listings to return. 0 = unlimited. |
| `maxPages` | integer | `5` | Maximum SERP pages per search source. StepStone returns 25 jobs/page. |
| `compact` | boolean | `false` | When true, each result contains only the most essential fields: jobId, title, company, location, salary, skills, url, description. Use in AI-agent and MCP workflows where token budgets matter. |
| `descriptionMaxLength` | integer | `0` | Truncate the description field to this many characters, appending '...' if truncated. 0 = no truncation. Pairs well with compact mode. |
| `outputFields` | array | — | Select subset of output fields. Supports dot notation: 'ceSalary.min'. If empty: all fields returned. |
| `datasetName` | string | — | Custom dataset name. Supports masks: {DATE} = YYYYMMDD, {TIME} = HHMMSS |
| `mode` | enum | `"full"` | full = return all jobs found. incremental = only return new/changed jobs (requires stateKey). |
| `incrementalMode` | boolean | `false` | Track changes between runs. Emits NEW, UPDATED, EXPIRED records. Requires stateKey. |
| `stateKey` | string | — | Stable identifier for the tracked search (e.g. "de-developer-berlin"). Required when incrementalMode is true. |
| `emitUnchanged` | boolean | `false` | In incremental mode, also emit records that haven't changed since last run. |
| `emitExpired` | boolean | `false` | In incremental mode, also emit records no longer found on StepStone. |
| `skipReposts` | boolean | `false` | Exclude listings detected as reposts of previously seen jobs. |
| `dedupStoreName` | string | — | Named KV store for cross-run dedup. Same name across runs = only new jobs pushed. |
| `dedupKey` | string | `"jobId"` | Field used as unique key for dedup (default: jobId) |
| `proxyConfiguration` | object | `{"useApifyProxy":true}` | Proxy configuration for reliable fetches. Default works for most cases. |
| `maxConcurrency` | integer | `5` | Maximum number of concurrent requests for SERP pages |
| `maxRequestRetries` | integer | `3` | Maximum number of retries per request |

---

## Output fields

Every listing returns the same 73-field schema. Missing values are `null` — never omitted.

- `jobId`
- `harmonisedId`
- `legacyJobId`
- `title`
- `company`
- `companyId`
- `companyLogo`
- `companyUrl`
- `companyWebsite`
- `companyEmployees`
- `companyIndustries`
- `companyBenefits`
- `companyAddress`
- `location`
- `postCode`
- `description`
- `descriptionHtml`
- `descriptionMarkdown`
- `descriptionLength`
- `textSections`
- `textSnippet`
- `textSnippetCleaned`
- `salaryText`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryPeriod`
- `unifiedSalary`
- `ceSalary`
- `locationDetail`
- `workFromHome`
- `workFromHomeLabel`
- `employmentType`
- `directApply`
- `industry`
- `skills`
- `labels`
- `isSponsored`
- `isTopJob`
- `isPartnershipJob`
- `isAnonymous`
- `isHighlighted`
- `companyRating`
- `partnership`
- `metaData`
- `section`
- `datePosted`
- `postedDaysAgo`
- `publishFromDate`
- `publishToDate`
- `validThrough`
- `canonicalUrl`
- `portalUrl`
- `applyUrl`
- `sourceUrl`
- `sourceDomain`
- `searchQuery`
- `scrapedAt`
- `detailFetched`
- `contentQuality`
- `contentHash`
- `similarJobCount`
- `isRepost`
- `originalPublishDate`
- `repostOfId`
- `repostDetectedAt`
- `changeType`
- `trackedHash`
- `firstSeenAt`
- `lastSeenAt`
- `previousSeenAt`
- `expiredAt`
- `stateKey`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "jobId": "88ee90e7097ee4e626590ecf654b59f25b9b34bfba7a245556330d474e6c3cab",
  "harmonisedId": "C4E3DE2A-2237-4E34-B09C-97FE01EC50FB",
  "legacyJobId": 13510716,
  "title": "Software Engineer (w/m/d)",
  "company": "Capgemini Deutschland GmbH",
  "companyId": 2692,
  "companyLogo": "https://www.stepstone.de/upload_DE/logo/9/logoCapgemini-Deutschland-GmbH-2692DE-2601160927.gif",
  "companyUrl": "https://www.stepstone.de/cmp/de/Capgemini-Deutschland-GmbH-2692/jobs.html",
  "companyWebsite": "http://www.capgemini.com/de-de/karriere/",
  "companyEmployees": "10000+",
  "companyIndustries": [
    "IT & Tech"
  ],
  "companyBenefits": []
}
```

*Truncated — full records contain 73 fields. See Output fields for the complete schema.*

**[Try StepStone.de Scraper - Germany Job Listings, Salary & Skills now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/stepstone-de-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.005 |
| Result | $0.0015 |

See the [actor on Apify](https://apify.com/blackfalcondata/stepstone-de-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape stepstone.de?**
Use this actor on Apify to extract structured data from stepstone.de. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get stepstone.de data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape stepstone.de?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check stepstone.de's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for listings extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data



- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [StepStone.de Scraper - Germany Job Listings, Salary & Skills](https://apify.com/blackfalcondata/stepstone-de-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 04*
