# catalyst-sdk.wiki

An implementation of the [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern (Karpathy, 2026) applied to the **Zoho Catalyst Java SDK v1**.

106 pages of SDK documentation → a persistent, interlinked knowledge base that an LLM builds, maintains, and queries. No RAG. No embeddings. Just markdown files, wikilinks, and a schema that tells the LLM how to be a disciplined wiki maintainer.

## the idea

Most approaches to LLMs + documents look like RAG: upload files, retrieve chunks at query time, generate an answer. The LLM re-discovers knowledge from scratch on every question. Nothing compounds.

This is different. The LLM reads each source document once, extracts the key information, and **compiles** it into a structured wiki — updating entity pages, revising topic summaries, cross-referencing concepts, flagging contradictions. The knowledge is built up incrementally, not re-derived on every query. A single source ingest might touch 10–15 wiki pages.

The wiki is the persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged. The synthesis already reflects everything that's been ingested.

> *"The tedious part of maintaining a knowledge base is not the reading or the thinking — it's the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims. LLMs don't get bored, don't forget to update a cross-reference, and can touch 15 files in one pass." — [Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)*

## architecture

Three layers, following the pattern:

```
raw/                        # layer 1: immutable source documents (LLM reads, never writes)
├── catalyst-java-sdk-*.md  #   8 SDK doc bundles scraped from official docs
├── catalyst-serverless-functions.md
└── assets/                 #   images, PDFs, data files

wiki/                       # layer 2: LLM-maintained knowledge base (LLM owns this)
├── index.md                #   master catalog — LLM reads this first on every query
├── overview.md             #   high-level synthesis across all 9 sources
├── log.md                  #   chronological record of every operation
├── sources/                #   one summary per ingested source (9 pages)
├── entities/               #   classes, products, services (15 pages)
├── concepts/               #   ideas, patterns, frameworks (42 pages)
├── comparisons/            #   side-by-side analyses
└── analyses/               #   query-derived pages filed back into the wiki

schema/                     # page templates and conventions
AGENTS.md                   # layer 3: the schema — operating instructions for the LLM
```

**Raw sources are immutable.** The LLM never modifies them. This is the source of truth.

**The wiki is the LLM's.** It creates pages, updates them when new sources arrive, maintains cross-references, keeps everything consistent. You read it; the LLM writes it.

**The schema is the configuration file.** It tells the LLM how the wiki is structured — page types, frontmatter conventions, naming rules, workflows for ingest/query/lint. It's what makes the LLM a disciplined wiki maintainer rather than a generic chatbot.

## operations

**Ingest.** Drop a source into `raw/`, tell the LLM to process it. It reads the source, creates a summary page, creates or updates entity and concept pages across the wiki, updates the index and log. One source might create 5 new pages and update 10 existing ones. The knowledge is compiled once.

**Query.** Ask a question. The LLM reads `wiki/index.md` to find relevant pages, reads them, synthesizes an answer with `[Source: filename]` citations. Good answers get filed back as new pages in `wiki/analyses/` — your explorations compound in the knowledge base just like ingested sources do.

**Lint.** Ask the LLM to health-check the wiki. It looks for contradictions between pages, orphan pages with no inbound links, important concepts that should have their own page but don't, stale content superseded by newer sources, and suggests new questions to investigate.

## what's in the wiki

The Zoho Catalyst Java SDK v1, fully ingested across **9 source bundles**:

| # | Source | Pages | What's Covered |
|---|--------|-------|----------------|
| 1 | SDK Overview | 3 | Initialization, scopes, class hierarchy, third-party OAuth |
| 2 | Authentication | 11 | User lifecycle: register, orgs, password, tokens, CRUD |
| 3 | Data Store | 10 | Table/column meta, row CRUD, pagination, bulk ops |
| 4 | Stratus | 17 | Object storage: buckets, upload/download, presigned URLs, versioning |
| 5 | Serverless Functions | 9 | 7 function types, runtimes, config, URL patterns |
| 6 | Cloud Scale (remaining) | 21 | File Store, ZCQL, Cache, Connections, Search, Mail, Push Notifications, Circuits, AppSail |
| 7 | Zia Services | 15 | OCR, AutoML, Face Analytics, Identity Scanner, Text Analytics, Image Moderation, Object Recognition, Barcode |
| 8 | SmartBrowz | 7 | Browser Grid management (5 ops), PDF & Screenshot generation (2 methods) |
| 9 | Job Scheduling + more | 20 | Job Scheduling (15), Pipelines (3), QuickML (1), Connectors (1) |

**106 documentation pages → 9 source summaries, 15 entity pages, 42 concept pages.** All cross-linked with `[[wikilinks]]`, all citing sources.

## quick start

```bash
git clone https://github.com/Aravindargutus/TestWiki.git
cd TestWiki
```

Point your LLM agent at `AGENTS.md` (the schema). The agent reads `wiki/index.md` first, then drills into relevant pages. This works well at this scale (~66 wiki pages) without any embedding infrastructure.

**Example queries:**
- *"How do I initialize the SDK with Client ID and Client Secret?"* → reads `wiki/concepts/third-party-sdk-integration.md`
- *"What's the difference between File Store and Stratus?"* → reads both concept pages, synthesizes a comparison
- *"Which operations are restricted by data center?"* → finds Circuits, Identity Scanner, AutoML, QuickML, Integration Functions

**To add a new source:**
1. Drop a markdown file into `raw/`
2. Tell the LLM: "ingest `raw/my-new-source.md`"
3. The LLM creates wiki pages, updates the index, updates the overview, appends to the log

## key entities

The SDK's class hierarchy, mirrored as entity pages in the wiki:

```
ZCProject                                  # root of everything
├── ZCUser + ZCSignUpData                  # authentication (11 ops)
├── ZCObject → ZCTable                     # data store (10 ops)
├── ZCStratus → ZCBucket → ZCObject        # object storage (17 ops)
├── ZCFile → ZCFolder                      # file store (5 ops)
├── ZCCache → ZCSegment                    # cache (5 ops)
└── ZCML                                   # all Zia AI/ML services
```

## data center restrictions

The wiki tracks where features don't work. This is the kind of cross-cutting knowledge that gets lost in per-page documentation but surfaces naturally in a compiled wiki:

| Component | Restriction |
|-----------|-------------|
| AutoML (Zia) | US data center only |
| Identity Scanner — Document Processing | IN data center only |
| Circuits | Not available in EU, AU, IN, JP, SA, CA |
| Integration Functions | US data center only |
| QuickML | Not available in JP, SA, CA |

## conventions

Every wiki page follows these rules (enforced by the schema):

- **YAML frontmatter** — title, type, created/updated dates, source list, tags
- **`[[wikilinks]]`** — cross-references between pages, liberally applied
- **`[Source: filename]`** — every factual claim traces back to a raw source
- **Kebab-case filenames** — `third-party-sdk-integration.md`, not `ThirdPartySDKIntegration.md`
- **Contradictions flagged explicitly** — when new data conflicts with existing wiki content

## known gaps

- NoSQL SDK pages failed to load — may not exist in Java SDK v1
- Node.js SDK and other language SDKs not yet covered
- Security Rules and API Gateway not yet documented
- Platform pricing, limits, and deployment mechanics unknown
- Roles ↔ scopes mapping needs deeper analysis
- Connectors vs Connections SDK disambiguation incomplete

These are tracked in `wiki/overview.md` under Knowledge Gaps. The lint operation can suggest which gaps to prioritize.

## why this works

The Catalyst Java SDK has 106 documentation pages across a dozen components. Asking an LLM to answer questions against that corpus via RAG means the LLM re-discovers the relevant pages every time. It doesn't know that Stratus replaced File Store. It doesn't know that Circuits and Integration Functions share the same data center restrictions. It doesn't know that `getInstance()` is a design pattern used across every component.

The wiki knows all of this. It was compiled once, incrementally, as each source was ingested. The LLM connected the dots as it went — and those connections persist for every future query.

The human's job was to curate sources, direct the analysis, and ask good questions. The LLM did the summarizing, cross-referencing, filing, and bookkeeping. That division of labor is the point.

## license

MIT
