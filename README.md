# Alum Finder — Automated Alumni Lead Discovery

Built by **Aadhya**, BITS Pilani, for the Committee of International Placements.

An n8n automation that finds alumni working at specific companies in specific countries, in a single click — built to speed up outreach for international placement drives. Instead of manually searching LinkedIn company-by-company, this workflow searches, filters, scores by seniority, and exports a ready-to-use spreadsheet of leads automatically.

## What it does

Given a list of target companies, the workflow:

1. **Searches** LinkedIn (via Google, through the Serper API) for BITS Pilani alumni at each company, scoped to a specific country
2. **Filters** results to only those genuinely mentioning BITS Pilani / Birla Institute of Technology and Science in the profile text
3. **Deduplicates** by LinkedIn profile URL, so the same person never appears twice
4. **Scores seniority** automatically — founders/C-suite score highest, down through Directors, VPs, Managers, Seniors, and Individual Contributors — so the most valuable contacts surface first
5. **Sorts** results by seniority score, highest first
6. **Exports** everything to a ready-to-share `.xlsx` spreadsheet

One click, and what used to be hours of manual LinkedIn searching turns into a sorted spreadsheet of hundreds of alumni leads.

## How to customize it

The entire search is driven by one place: the **HTTP Request** node. Its query is:

```
site:linkedin.com/in ("BITS Pilani" OR "Birla Institute of Technology and Science") "{{company}}" Singapore
```

To search a different country, just change `Singapore` to any other country name in that query. To search different companies, edit the company list in the first **Code in JavaScript** node — it's just a plain array:

```js
{ json: { company: "Databricks" } },
{ json: { company: "Snowflake" } },
// add or remove companies here
```

That's it — no other part of the workflow needs to change. Swap the country, swap the company list, run it again.

## Sample output

Running this workflow produces a spreadsheet like the one below (`alum-finder-sample-output.xlsx` in this repo). Note: the names, profile links, and snippets in this sample are **entirely fictional**, created only to demonstrate the output format — the real workflow's actual output contains genuine alumni data, which isn't published here for privacy reasons.

| Company | AlumName | Post | SeniorityScore | SeniorityLevel |
|---|---|---|---|---|
| Databricks | Arjun Mehta | Co-founder & CEO | 20 | Executive |
| Snowflake | Priya Raghunathan | Managing Director | 15 | Executive |
| Palantir | Rohan Deshpande | Director of Engineering | 12 | Director |
| MongoDB | Sneha Iyer | Vice President, Product | 10 | VP |
| Confluent | Karan Bhatia | Head of Platform Engineering | 8 | Leadership |
| Cloudflare | Ananya Krishnan | Engineering Manager | 6 | Manager |

## Setup

⚠️ **This workflow requires your own Serper API key to run.** No API key is included in this repository — you must provide your own for the workflow to work.

1. Import `alum-finder.json` into your n8n instance
2. Sign up at [serper.dev](https://serper.dev) and get your own API key (they offer a free tier)
3. Open the **HTTP Request** node in n8n, and paste your key into the `X-API-KEY` header field, replacing the placeholder `YOUR_SERPER_API_KEY_HERE`
4. Edit the company list (in the **Code in JavaScript1** node) and the target country (in the **HTTP Request** node's query) as needed
5. Click **Execute workflow**
6. Download the generated `.xlsx` file from the final node's output

## Tech / tools used

- **n8n** — workflow automation
- **Serper API** — Google search access
- **JavaScript (Code nodes)** — filtering, deduplication, seniority scoring
- Built and refined with **Claude (Anthropic)** as a development assistant for debugging the JavaScript logic and structuring the workflow

## Why I built this

As part of BITS Pilani's International Placements Committee, a recurring bottleneck is finding alumni at target companies abroad to reach out to for referrals and placement drives. This automation turns a manual, repetitive research task into a one-click process that scales to any company list or country.
