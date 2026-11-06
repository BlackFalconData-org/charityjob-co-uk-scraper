# CharityJob UK Scraper [💰$1.15]

Extract structured data from [charityjob.co.uk](https://charityjob.co.uk) — charityjob.co.uk — records. Every record carries email, salary and address the source publishes. Address and city is included where the source lists it. Structured rows, ready to import into a CRM or a spreadsheet.

**[CharityJob UK Scraper [💰$1.15] on Apify →](https://apify.com/blackfalcondata/charityjob-co-uk-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 🏢 employer type, 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, salary data, employer profiles, contact information for each job.

**Incremental mode** — Only get new or changed jobs since your last run. Content hash per job — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how jobs evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N jobs (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every job returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured jobs from charityjob.co.uk on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor jobs, track trends, and analyze market dynamics with structured, deduplicated data from charityjob.co.uk.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on charityjob.co.uk to inform pricing decisions, hiring plans, or candidate negotiations.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per job is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "fundraising manager",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job title or keywords, e.g. "fundraising manager", "support worker", "trusts and foundations". Leave empty and use a Start URL to browse a filtered listing instead. |
| `location` | string | — | UK town, city, region or postcode, e.g. "London", "Greater Manchester", "EH1". Leave empty to search the whole UK. |
| `days` | integer | — | Only jobs posted on CharityJob within this many days (for example 7 = the last week). Leave empty for any age. The site applies this filter itself, so older jobs are never fetched or billed. A run with this filter never marks jobs as expired in incremental mode. |
| `cause` | array | — | Only jobs at charities working on these causes (pick one or several; a job matches if it fits any). The site applies this filter itself, so other jobs are never fetched or billed. Leave empty for all causes. |
| `workplace` | array | — | Only jobs with this working pattern (on-site, hybrid or remote; several allowed). Leave empty for any. |
| `contractType` | array | — | Only jobs with this contract type (several allowed; a job matches if it fits any). Leave empty for any. |
| `employerType` | enum | `"any"` | Direct employers only leaves out recruitment agencies. The site applies this filter itself. |
| `minSalary` | integer | — | Only jobs paying at least this yearly salary in pounds, e.g. 30000. Leave empty for no minimum. |
| `maxSalary` | integer | — | Only jobs paying at most this yearly salary in pounds, e.g. 60000. Leave empty for no maximum. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw search URLs from the target site. Each URL becomes its own search task; results are merged and deduped by job ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch full job details. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Which description representations to emit. `all` keeps descriptionText + descriptionHtml + descriptionMarkdown (default). `text` / `html` / `markdown` keep only one variant — typically halves output size. |
| `excludeEmptyFields` | boolean | `false` | Drop null / empty-string / empty-array fields (and fully-null nested objects like socialProfiles) from each output record. Off by default to preserve dataset schema compatibility; turn on to slim output for LLM / agent consumption. |
| `enrichEmails` | boolean | `false` | Reads each hiring organisation's own website (the address on its CharityJob profile): its homepage and a few contact pages, and adds contactWebsite, contactEmail and contactEmails to every job of that organisation. Best effort: not every organisation lists a website or a public email. Free (no extra charge) but makes runs slower. Needs Include Full Details, because the website comes from the organisation profile. |
| `hiringMarketAnalysis` | boolean | `false` | Adds one summary row per role and UK region found in this run's results: posting count, annual GBP salary band (min, median, max over postings that state a yearly salary), top 5 employers, top 5 causes, employment-type mix and remote share. Computed from the scraped postings, no AI. Summary rows have recordType "marketSnapshot"; job rows are unchanged. Each summary row is a dataset item and is billed like one. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (keywords, locations, start URLs) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | Also emit jobs that have not changed since the last run (incrementalMode only). |
| `emitExpired` | boolean | `false` | Also emit jobs that have disappeared since the last run (incrementalMode only). |
| `skipReposts` | boolean | `false` | When incremental, skip jobs whose content matches an expired job from a prior run (cross-run repost detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED jobs. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of jobs included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every job returns the same 131-field schema. Missing values are `null` — never omitted.

- `jobId`
- `jobUrl`
- `portalUrl`
- `jobSlug`
- `jobReference`
- `title`
- `isVolunteerRole`
- `organisation`
- `company`
- `organisationSlug`
- `hasOrganisationProfile`
- `organisationUrl`
- `organisationType`
- `organisationSize`
- `organisationBlurb`
- `partnerId`
- `logoUrl`
- `companyLogo`
- `location`
- `locationName`
- `workplace`
- `isRemote`
- `city`
- `region`
- `postcode`
- `countryCode`
- `addressLocality`
- `addressRegion`
- `jobLocalities`
- `jobRegions`
- `jobLocationType`
- `salaryRaw`
- `salaryText`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryPeriod`
- `hours`
- `contractType`
- `employmentType`
- `employmentTypes`
- `causes`
- `categories`
- `tags`
- `postedOn`
- `refreshedOn`
- `listedOrRefreshedOn`
- `postedRelative`
- `datePosted`
- `postedAt`
- `closingDate`
- `closingAt`
- `validThrough`
- `closingBadge`
- `isTopJob`
- `isFeatured`
- `isExpiredJob`
- `isFutureJob`
- `activelyInterviewing`
- `applyLabel`
- `applyType`
- `isQuickApply`
- `applicationUrl`
- `agencyContactRestriction`
- `attachments`
- `educationRequirements`
- `experienceRequirements`
- `applicantLocationRequirements`
- `shortDescription`
- `descriptionText`
- `descriptionHtml`
- `charityWebsiteUrl`
- `linkedinUrl`
- `facebookUrl`
- `twitterUrl`
- `youtubeUrl`
- `instagramUrl`
- `tiktokUrl`
- `orgHeadquarters`
- `orgMission`
- `orgAboutText`
- `orgAboutHtml`
- `orgProfileId`
- `orgPaidJobCount`
- `orgVolunteerJobCount`
- `orgBannerImage`
- `orgMedia`
- `companyBenefits`
- `companyEmail`
- `companyPhone`
- `companyAddressStreet`
- `companyAddressCity`
- `companyAddressPostcode`
- `companyAddressCountry`
- `companyActiveJobsCount`
- `companyActiveJobs`
- `searchQuery`
- `sourcePlatform`
- `sourceUrl`
- `resultPosition`
- `scrapedAt`
- `descriptionMarkdown`
- `extractedEmails`
- `extractedPhones`
- `extractedUrls`
- `socialProfiles`
- `contactWebsite`
- `contactEmail`
- `contactEmails`
- `recordType`
- `role`
- `postingCount`
- `salarySampleSize`
- `salaryMedian`
- `topEmployers`
- `employmentTypeMix`
- `topCauses`
- `remoteShare`
- `generatedAt`
- `source`
- `contentQuality`
- `detailFetched`
- `contentHash`
- `changeType`
- `firstSeenAt`
- `lastSeenAt`
- `previousSeenAt`
- `expiredAt`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`

---

## Sample output

One object per job. Here is a real example from a production run:

```json
{
  "jobId": "1089665",
  "jobUrl": "https://www.charityjob.co.uk/jobs/target-ovarian-cancer/events-fundraising-manager/1089665",
  "portalUrl": "https://www.charityjob.co.uk/jobs/target-ovarian-cancer/events-fundraising-manager/1089665",
  "jobSlug": "events-fundraising-manager",
  "jobReference": "65464437798",
  "title": "Events Fundraising Manager",
  "isVolunteerRole": false,
  "organisation": "Target Ovarian Cancer",
  "company": "Target Ovarian Cancer",
  "organisationSlug": "target-ovarian-cancer",
  "hasOrganisationProfile": true,
  "organisationUrl": "https://www.charityjob.co.uk/organisation/target-ovarian-cancer"
}
```

*Truncated — full records contain 131 fields. See Output fields for the complete schema.*

**[Try CharityJob UK Scraper [💰$1.15] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/charityjob-co-uk-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.001152 |

See the [actor on Apify](https://apify.com/blackfalcondata/charityjob-co-uk-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape charityjob.co.uk?**
Use this actor on Apify to extract structured data from charityjob.co.uk. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get charityjob.co.uk data as JSON, CSV, or Excel?**
The actor writes each job to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape charityjob.co.uk?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check charityjob.co.uk's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each job gets a content hash. On subsequent runs, only new or changed jobs are emitted — saving time, compute, and storage. Expired jobs can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [CharityJob UK Scraper [💰$1.15]](https://apify.com/blackfalcondata/charityjob-co-uk-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
