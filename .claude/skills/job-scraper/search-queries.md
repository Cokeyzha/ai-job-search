# Search Queries for Job Scraper

<!-- SETUP: Customize these queries based on your skills, target roles, and location -->

## Installed portal CLIs (primary for `/scrape`)

`/scrape` discovers every portal skill under `.agents/skills/*/SKILL.md` and runs its CLI first. Shipped country-agnostic CLIs include `linkedin-search` and `freehire-search`; Danish demos and any skill you add with `/add-portal` are included the same way. You do **not** need a matching `site:` line below for those CLIs to run.

The `site:` query templates in this file are the **WebSearch fallback** — for portals without a CLI, company career pages, or when a CLI fails.

## Search Sites

Primary (your market's job boards - scaffold one with `/add-portal`):
- **[YOUR_JOB_BOARD]** - your market's largest general job board
- **linkedin.com/jobs** - LinkedIn job listings (filter: [YOUR_COUNTRY] / [YOUR_CITY]); also covered by `linkedin-search` CLI
- **[YOUR_INDUSTRY_JOB_BOARD]** - a niche/industry board for your field (optional)
- **[YOUR_ADDITIONAL_JOB_BOARD]** - another major board for your market (optional)

Secondary (company career pages via Google):
- Direct Google searches with `site:` filters for known target companies

## Query Categories

Queries are grouped by priority. Each query should be combined with your location terms (e.g. your city, region, or metro area) where the site supports it.

### Priority 0: Target Companies (B2B Enterprise List)

A named target-company list, ranked by priority (highest first). Search these before the generic role-based categories below.

**Method, per company:**
1. Try `freehire-search` first — it aggregates ~50 ATS platforms (Greenhouse, Lever, etc.) and many of these companies run on one of them:
   ```
   bun run .agents/skills/freehire-search/cli/src/cli.ts search -q "<role keyword> <company>" --limit 10 --format table
   ```
   Or, once you know a company's freehire slug from a prior hit, filter directly with `--company <slug>`.
2. If freehire returns nothing for a company (common for ones running a custom/enterprise ATS — e.g. Workday, SuccessFactors, iCIMS), fall back to WebSearch:
   ```
   "<company>" careers "<role keyword>" [YOUR_CITY]
   ```
3. Also try the `linkedin-search` CLI scoped to that company where useful (LinkedIn's `--query` can include the company name).

**Rotation note:** 100 companies is too many to query in full on every `/scrape` run — it burns time and API budget on companies with no open roles that day. Rotate a subset per run (e.g. ~10-15), cycling through the ranked list across successive `/scrape` calls rather than querying all 100 every time. Weight higher-ranked companies to appear more often. If the user says "/scrape companies" or names a specific company, prioritize that over the rotation.

Target companies:

1. Microsoft — Cloud, productivity, enterprise AI
2. Amazon Web Services — Cloud infrastructure and AI
3. Google Cloud — Cloud, data and AI
4. NVIDIA — AI infrastructure and enterprise computing
5. Oracle — Databases, cloud and enterprise applications
6. SAP — ERP and enterprise operations
7. Salesforce — CRM and enterprise applications
8. ServiceNow — Enterprise workflow automation
9. IBM — Hybrid cloud, AI and consulting technology
10. Adobe — Enterprise content and marketing technology
11. Intuit — Business financial software
12. Workday — HR and financial management
13. Snowflake — Cloud data platform
14. Databricks — Data and AI platform
15. Atlassian — Engineering and work-management software
16. Stripe — Payments infrastructure
17. Visa — Payments network and financial technology
18. Mastercard — Payments, data and financial technology
19. Fiserv — Banking and merchant technology
20. HubSpot — CRM and marketing technology
21. OpenAI — Enterprise generative AI
22. Anthropic — Enterprise generative AI
23. Cisco — Networking and enterprise infrastructure
24. Dell Technologies — Enterprise hardware and infrastructure
25. Hewlett Packard Enterprise — Hybrid cloud and infrastructure
26. Broadcom — Semiconductors and enterprise infrastructure
27. Shopify — Commerce infrastructure
28. Block / Square — Merchant payments and commerce
29. PayPal — Digital and merchant payments
30. Adyen — Enterprise payment processing
31. Toast — Restaurant commerce platform
32. Datadog — Cloud monitoring and observability
33. MongoDB — Developer data platform
34. Confluent — Data streaming infrastructure
35. GitLab — Software development platform
36. GitHub — Developer collaboration and AI coding
37. Twilio — Communications APIs
38. Zoom — Enterprise communications
39. DocuSign — Agreement management
40. Figma — Collaborative product design
41. Canva — Enterprise visual communication
42. Rippling — HR, IT and finance platform
43. Ramp — Corporate cards and spend management
44. Brex — Corporate financial platform
45. Plaid — Financial-data infrastructure
46. BILL — SMB financial operations
47. Gusto — Payroll and HR technology
48. Navan — Business travel and expense management
49. Deel — Global payroll and workforce management
50. Coupa — Business spend management
51. Celonis — Process intelligence
52. UiPath — Enterprise automation
53. Automation Anywhere — Robotic and agentic automation
54. Appian — Low-code process automation
55. Pegasystems — Enterprise workflow and decisioning
56. Klaviyo — B2B commerce marketing platform
57. Braze — Customer-engagement platform
58. Zendesk — Customer-service software
59. Intercom — AI customer-service platform
60. Gong — Revenue intelligence
61. Qualtrics — Experience-management software
62. Notion — Knowledge and productivity platform
63. Airtable — Collaborative application platform
64. Miro — Visual collaboration
65. Asana — Work management
66. monday.com — Work-management platform
67. Smartsheet — Enterprise work management
68. ClickUp — Productivity and project management
69. Dropbox — Content collaboration
70. Box — Enterprise content management
71. Zapier — Business workflow automation
72. Workato — Enterprise integration and automation
73. dbt Labs — Analytics engineering
74. Dataiku — Enterprise AI and analytics
75. DataRobot — Enterprise AI platform
76. Glean — Enterprise search and AI
77. Hugging Face — AI model and developer platform
78. CoreWeave — AI cloud infrastructure
79. Together AI — AI infrastructure and model platform
80. Samsara — Connected operations and IoT
81. ServiceTitan — Home-services operating platform
82. Procore — Construction technology
83. Autodesk — Design and engineering software
84. PTC — Industrial software and IoT
85. Trimble — Construction, geospatial and logistics technology
86. Guidewire — Insurance software
87. nCino — Cloud banking platform
88. Icertis — Contract lifecycle management
89. Veeva Systems — Life-sciences cloud software
90. Epic Systems — Healthcare information systems
91. Tempus AI — Clinical data and precision medicine
92. Datavant — Healthcare data connectivity
93. Waystar — Healthcare payments and revenue cycle
94. Abridge — Clinical AI documentation
95. Benchling — Life-sciences R&D software
96. Cedar — Healthcare billing and patient financial experience
97. Innovaccer — Healthcare data and care-management platform
98. Komodo Health — Healthcare data and analytics
99. athenahealth — Provider and revenue-cycle technology
100. R1 RCM — Healthcare revenue-cycle technology

### Priority 1: [YOUR_PRIMARY_ROLE_TYPE]

These match your strongest and most desired career direction.

```
site:[YOUR_JOB_BOARD] "[YOUR_PRIMARY_JOB_TITLE]" [YOUR_CITY]
site:[YOUR_JOB_BOARD] "[YOUR_KEY_SKILL]" [YOUR_CITY]
site:linkedin.com/jobs "[YOUR_PRIMARY_JOB_TITLE]" [YOUR_COUNTRY]
```

### Priority 2: [YOUR_DOMAIN_EXPERTISE]

These match your domain expertise.

```
site:[YOUR_JOB_BOARD] [YOUR_DOMAIN_KEYWORD_1] [YOUR_CITY] OR [YOUR_REGION]
site:[YOUR_JOB_BOARD] [YOUR_DOMAIN_KEYWORD_2] [YOUR_COUNTRY]
site:linkedin.com/jobs [YOUR_DOMAIN_KEYWORD_1] [YOUR_CITY] [YOUR_COUNTRY]
```

### Priority 3: [YOUR_ADJACENT_ROLE_TYPE]

Adjacent roles you could pivot into.

```
site:[YOUR_JOB_BOARD] "[YOUR_ADJACENT_TITLE_1]" [YOUR_KEY_SKILL] [YOUR_CITY]
site:[YOUR_JOB_BOARD] "[YOUR_ADJACENT_TITLE_2]" [YOUR_KEY_SKILL] [YOUR_CITY]
```

### Priority 4: Broader Technical / Consulting

Wider net for general technical roles.

```
site:[YOUR_JOB_BOARD] [YOUR_KEY_SKILL] developer [YOUR_CITY]
site:linkedin.com/jobs "[YOUR_KEY_SKILL] developer" [YOUR_CITY]
site:[YOUR_JOB_BOARD] "technical consultant" [YOUR_DOMAIN] [YOUR_CITY]
```

## Location Filter

When evaluating results, verify the job location is within reasonable commute distance from your home. Define acceptable areas:
- [YOUR_CITY] and surrounding areas
- [ACCEPTABLE_AREA_1]
- [ACCEPTABLE_AREA_2]
- [BORDERLINE_AREA] (borderline - ~X min by transit)
- [TOO_FAR_AREA] (too far)

## Date Filter

Only include jobs posted within the last 14 days, or with an application deadline that has not yet passed. If a posting date cannot be determined, include it but flag as "date unknown".

## Adapting Queries

If the user specifies a focus area, select queries from the matching category and also generate 2-3 custom queries for that focus. For example:
- "/scrape [focus_area]" -> relevant category queries + custom focus-specific queries
