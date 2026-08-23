# Lead Intelligence Source Mesh Atlas

Status: provider/source discovery record. No runtime routing authority.

Architecture companion: `Ginkgo-Core/ai-stack#34` / `architecture/lead-intelligence-source-mesh-v1.md`.

## Strategic decision

The ideal Lead Intelligence system is not one lead database, one scraper, or one MCP server. It is a governed mesh of complementary evidence sources.

```text
Google Places = local/geospatial business discovery
Exa = semantic web discovery
Firecrawl = web extraction and site enrichment
Apollo = B2B account + buying-committee intelligence
Hunter = email intelligence and verification
CNPJ Intelligence = Brazilian corporate identity
Twilio Lookup = phone validation
Playwright MCP = bounded browser exploration
PDL = deep enrichment
Wappalyzer/BuiltWith = technographics
Similarweb = digital/traffic signals
Apify = long-tail specialized extractors
Bright Data = high-cost/heavy web data path
LinkedIn official = authorized professional graph path
```

Ginkgo owns canonical lead identity, qualification, evidence, deduplication, source policy, persistence and lifecycle. None of these providers owns the Lead Intelligence skill.

## API vs MCP

Recommended doctrine:

```text
API = deterministic production data plane
MCP = bounded agentic tool plane
```

Use typed APIs for recurring high-volume lookups, cost accounting, retries and reproducible tests. Use MCP when agents need exploratory research, browser interaction, tool discovery or adaptive navigation.

A 200-accepted-lead recurring job should not require an LLM/MCP round-trip for every deterministic lookup.

## P0 portfolio

| Provider/source | Interface | Primary role | Default posture |
|---|---|---|---|
| Google Places API New | API | Local/geospatial business discovery | Discovery sensor, source-specific terms gate |
| Firecrawl | API + MCP | Search/scrape/crawl/extract/site enrichment | API production candidate; MCP research candidate |
| Exa | API + MCP | Semantic company/people/web discovery | Complement Google/Apollo discovery |
| Apollo | API | B2B accounts, organizations, decision makers | Account -> buying committee |
| Hunter | API | Domain/email find + verify + enrichment | Contact enrichment after org/domain resolution |
| Receita CNPJ Open Data | Dataset | Brazilian company identity | Prefer local Postgres ingestion |
| BrasilAPI CNPJ | API | Secondary Brazilian company lookup | Convenience/fallback lookup |
| Twilio Lookup v2 | API | Phone validation/E.164/line signals | Contact validation, not WhatsApp proof |
| Playwright MCP | MCP | Bounded browser read/navigation | READ-only by default |

### Why this is the minimum powerful stack

It covers independent axes of uncertainty:

```text
Does the company exist locally?         -> Places / CNPJ
Is it semantically relevant?            -> Exa / Apollo
What does its website actually say?     -> Firecrawl
Who are the likely decision makers?     -> Apollo
Can we find/verify business email?       -> Hunter
Is the phone syntactically/technically valid? -> Twilio
Can an agent inspect a page when APIs fail?   -> Playwright MCP
```

No single provider answers all of these reliably.

## P1 portfolio

| Provider/source | Interface | Role | Use condition |
|---|---|---|---|
| People Data Labs | API | Deep people/company enrichment | Only after cheaper evidence is insufficient |
| Wappalyzer | API | Technographics | ICP/automation-opportunity scoring |
| BuiltWith | API | Technographics/technology lists | Alternative/complement to Wappalyzer |
| Similarweb | API | Traffic/engagement/channel signals | Higher-value account scoring |
| Tavily | API + MCP | Secondary web research | Same-semantics search fallback candidate |
| Apify | API + MCP | Specialized Actors/extractors | Long-tail source experiments under per-source policy |

### Technographics as commercial intelligence

Useful signals include:
- Shopify / WooCommerce;
- WordPress;
- HubSpot / Salesforce / RD Station;
- Zendesk / Intercom / Chatwoot;
- Google Analytics / Meta Pixel;
- payment/e-commerce infrastructure;
- chat/support technologies.

Example of a stronger target definition:

```text
Brazilian Shopify company
+ meaningful traffic
+ public business contact
+ paid-media signals
+ no detected conversational automation
+ ICP score >= 60
```

This is much more actionable than a generic list of "e-commerce companies".

## P2 portfolio

| Provider/source | Interface | Role | Reason not default |
|---|---|---|---|
| Bright Data | API + MCP | Heavy-duty web/search/browser/datasets | Cost and overlap with P0/P1 |
| LinkedIn / Sales Navigator official | Official integration/API/product | Professional graph | Restricted/authorized access paths |

Future P2 candidates can include funding/startup databases, global registries, job/vacancy signals, ad-intent datasets and customer-owned first-party data.

## Source waterfall

Do not enrich every raw row with every paid provider.

Recommended cost-aware waterfall:

```text
1. discover organization
2. establish canonical domain/CNPJ/place identity
3. apply local/free/public enrichment
4. validate available contact evidence
5. compute preliminary ICP score
6. call paid deep enrichment only for promising candidates
7. final qualification
8. deduplicate
9. persist accepted lead
```

Example provider waterfall:

```text
Google Places / Exa / Apollo
-> Firecrawl website evidence
-> CNPJ local database
-> Hunter
-> Twilio
-> technographics
-> PDL only if needed
-> Similarweb only for high-value scoring
```

## Source Router model

Provider selection belongs to the Source Router, not the caller or the agent prompt.

Inputs to routing can include:
- semantic intent;
- geography;
- account/person/place target type;
- required output fields;
- Source Policy eligibility;
- accepted-lead yield history;
- marginal enrichment uplift;
- cost;
- rate limit;
- provider health;
- freshness;
- latency.

Forbidden caller choices:
- vendor/provider;
- adapter;
- MCP server;
- Apify Actor;
- scraper;
- proxy;
- browser profile;
- credential;
- anti-bot strategy.

## Google Places posture

Google Places is valuable as an official local discovery sensor, but Ginkgo should not model it as the unrestricted canonical lead database.

Recommended design:

```text
Places search
-> place_id + source evidence
-> domain/company resolution
-> independent enrichment
-> canonical Ginkgo lead record
```

Persist/cache only fields and durations permitted by the current terms and application design. Source Policy must carry the exact decision.

## Firecrawl posture

Firecrawl is the preferred first web extraction provider because it can expose search/scrape/crawl/extract primitives behind one adapter family.

Production preference:

```text
Firecrawl API
```

Agent research preference:

```text
Firecrawl MCP
```

Firecrawl does not determine ICP scoring, WhatsApp status, source eligibility, deduplication or CRM persistence.

## Exa posture

Exa is a discovery complement, not a Firecrawl replacement.

```text
Exa = find relevant web entities/pages
Firecrawl = extract evidence from selected web targets
```

Use Exa for semantic discovery where city/category lookup or B2B database filters are insufficient.

## Apollo posture

Apollo is most valuable as an account/people graph.

```text
canonical organization
-> Apollo
-> candidate decision makers
-> role/seniority evidence
-> contact enrichment waterfall
```

Do not copy provider-specific entity identity into the public Ginkgo contract.

## Hunter posture

Hunter is a natural P0 email layer because it supports finder/search/verification flows and can expose evidence/source information.

Preferred semantic sequence:

```text
organization/domain known
-> find business email candidates
-> verify
-> retain evidence/confidence
```

## Brazilian CNPJ Intelligence

For Brazilian leads, a local corporate identity database should be a core advantage.

Target architecture:

```text
Receita Federal open datasets
-> scheduled ingestion
-> normalized CNPJ tables
-> indexed local Postgres
-> sales.lead.enrich.corporate_registry
```

Benefits:
- deterministic joins;
- low marginal lookup cost;
- CNPJ/CNAE/status/municipality evidence;
- improved deduplication;
- better ICP segmentation.

BrasilAPI remains a useful convenience source but should not replace source lineage or the local ingestion strategy.

## Contact validation

Twilio Lookup can strengthen telephone validation and line-type evidence.

Canonical rule stays:

```text
phone VALID
!=
WhatsApp VALID
```

Do not send a message or place a call merely to test channel existence inside Lead Intelligence.

## MCP portfolio

Recommended MCP candidates:

```text
Firecrawl MCP
Exa MCP
Playwright MCP
Tavily MCP
Apify MCP
Bright Data MCP
```

MCP-specific gates:
- server identity/version;
- exact tool allowlist;
- argument schema;
- credential scope;
- network/source scope;
- risk classification;
- max steps/deadline/cost;
- evidence mapping;
- prompt/tool-output injection tests;
- independent Source Policy decision.

MCP server trust is never transitive. A server exposing a tool does not make the target source authorized.

## Apify posture

Apify is useful as a universal experimental adapter because it offers specialized Actors without requiring Ginkgo to integrate every extractor as a first-class vendor.

Correct model:

```text
Ginkgo capability
-> Source Router
-> Apify adapter
-> approved Actor binding
```

Incorrect model:

```text
agent discovers arbitrary Actor
-> executes it
-> persists results
```

Each Actor/source combination needs its own policy and contract evidence.

## Bright Data posture

Use as an escalation/high-value provider after P0/P1 paths are insufficient and the target source is explicitly allowed.

It should not silently become universal fallback because that would:
- raise cost;
- duplicate Firecrawl/Exa/Apify paths;
- make source governance harder;
- reduce determinism.

## LinkedIn posture

LinkedIn/Sales Navigator is strategically valuable, but Ginkgo should depend only on authorized/official integration paths appropriate to the account/product.

Unofficial scraping is not a foundation dependency.

## Quality metrics

Evaluate providers by business outcome, not rows returned.

Primary metric:

```text
cost / accepted evidence-backed policy-eligible lead
```

Track:
- raw candidates;
- canonical organizations;
- accepted-lead yield by source;
- marginal uplift from each enrichment provider;
- false-positive/contact-validation rate;
- duplicate rate;
- cost per accepted lead;
- source-policy blocks;
- rate-limit/provider failure rate;
- evidence completeness;
- freshness.

Providers that increase row volume without increasing accepted-lead yield should lose router priority.

## Recommended implementation order

### P0-A
1. local CNPJ Intelligence ingestion/adapter;
2. production Firecrawl transport;
3. Exa discovery adapter.

### P0-B
4. Google Places source-specific adapter;
5. Apollo account/people adapter;
6. Hunter email adapter;
7. Twilio Lookup adapter.

### P0-C
8. governed Playwright MCP binding;
9. Source Router yield/cost scoring;
10. multi-source reference E2E.

### P1
11. Wappalyzer/BuiltWith technographics;
12. PDL fallback enrichment;
13. Similarweb high-value scoring;
14. Tavily same-semantics search fallback evaluation;
15. Apify per-Actor governed adapter framework.

### P2
16. Bright Data enterprise path;
17. LinkedIn official integration when access is available.

## Multi-source E2E acceptance

The reference E2E should prove:
- two or more discovery sources can converge to one canonical organization;
- CNPJ/domain/place/person identities remain distinct but linked;
- every material field retains provenance;
- blocked provenance cannot be laundered during merge;
- paid enrichments honor max-cost policy;
- provider failure does not cross semantic boundaries silently;
- no MCP/tool can change router authority;
- no outreach occurs;
- accepted-lead metrics reconcile.

## Doctrine

```text
PROVIDER EXISTS
!= SOURCE AUTHORIZED
!= ADAPTER VALIDATED
!= ROUTING ELIGIBLE
!= PRODUCTION ELIGIBLE
```

and:

```text
MORE ROWS
!= BETTER LEADS
```

The objective is a high-confidence evidence graph around each organization, not the largest CSV.
