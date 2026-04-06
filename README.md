# catalyst-sdk.wiki

An LLM-maintained knowledge base for the **Zoho Catalyst Java SDK v1** — 106 documentation pages distilled into a structured, interlinked wiki that an LLM can read, query, and extend.

Inspired by the [Karpathy](https://github.com/karpathy) approach to building things from scratch: no bloat, everything in plain Markdown, every claim traced to a source.

## what is this

Zoho Catalyst is a serverless Backend-as-a-Service (BaaS) platform. Its Java SDK wraps REST APIs into a class hierarchy rooted at `ZCProject`, supporting Java 8, 11, and 17. This repo is a machine-readable knowledge base covering the **entire** SDK surface area.

**106 pages** of official documentation → **9 source summaries**, **15 entity pages**, **42 concept pages**, fully cross-linked.

```
raw/                        # immutable source documents (never modified)
├── catalyst-java-sdk-*.md  # 8 SDK doc bundles
├── catalyst-serverless-functions.md
└── assets/

wiki/                       # LLM-maintained knowledge base
├── index.md                # master catalog — LLM reads this first
├── overview.md             # high-level synthesis across all sources
├── log.md                  # chronological record of all operations
├── sources/                # one summary per ingested source (9 pages)
├── entities/               # classes, products, services (15 pages)
├── concepts/               # ideas, patterns, frameworks (42 pages)
├── comparisons/            # side-by-side analyses (empty — PRs welcome)
└── analyses/               # query-derived pages (empty — PRs welcome)

schema/                     # page templates
AGENTS.md                   # operating instructions for the LLM
```

## coverage

The SDK is fully ingested across **9 source bundles**:

| # | Source | Pages | What's Covered |
|---|--------|-------|----------------|
| 1 | SDK Overview | 3 | Init, scopes, class hierarchy, third-party OAuth |
| 2 | Authentication | 11 | User lifecycle: register, orgs, password, tokens, CRUD |
| 3 | Data Store | 10 | Table/column meta, row CRUD, pagination, bulk ops |
| 4 | Stratus | 17 | Object storage: buckets, upload, download, presigned URLs, versioning |
| 5 | Serverless Functions | 9 | 7 function types, runtimes, config, URLs |
| 6 | Cloud Scale (remaining) | 21 | File Store, ZCQL, Cache, Connections, Search, Mail, Push, Functions, Circuits, AppSail |
| 7 | Zia Services | 15 | OCR, AutoML, Face Analytics, Identity Scanner, Text Analytics, Image Moderation, Object Recognition, Barcode |
| 8 | SmartBrowz | 7 | Browser Grid (5 ops), PDF & Screenshot (2 methods) |
| 9 | Job Scheduling + more | 20 | Job Scheduling (15), Pipelines (3), QuickML (1), Connectors (1) |

**Total: 106 pages → 66 wiki pages**

## quick start

Clone and ask your LLM questions:

```bash
git clone https://github.com/Aravindargutus/TestWiki.git
cd TestWiki
```

Point your LLM at `AGENTS.md` (the operating schema) and `wiki/index.md` (the master catalog). The LLM will:
1. Read the index to find relevant pages
2. Read those pages for detail
3. Synthesize an answer with `[Source: filename]` citations
4. Optionally file the answer back into `wiki/analyses/`

### example queries

```
> How do I initialize the Java SDK with Client ID and Client Secret?
  → reads wiki/concepts/third-party-sdk-integration.md, raw source

> What's the difference between File Store and Stratus?
  → reads wiki/concepts/file-store.md, wiki/concepts/stratus.md

> Which SDK operations are restricted by data center?
  → reads wiki/concepts/circuits.md, identity-scanner.md, quickml.md
```

## key entities

The SDK's class hierarchy, mirrored in entity pages:

```
ZCProject                          # wiki/entities/zcproject.md
├── ZCUser + ZCSignUpData          # authentication
├── ZCObject → ZCTable             # data store
├── ZCStratus → ZCBucket → ZCObject(Stratus)  # object storage
├── ZCFile → ZCFolder              # file store
├── ZCCache → ZCSegment            # cache
└── ZCML                           # all Zia AI/ML services
```

Plus platform entities: [[zoho-catalyst]], [[smartbrowz]].

## data center restrictions

Not everything works everywhere. The wiki tracks these:

| Component | Restriction |
|-----------|-------------|
| AutoML (Zia) | US data center only |
| Identity Scanner — Document Processing | IN data center only |
| Circuits | Not available in EU, AU, IN, JP, SA, CA |
| Integration Functions | US data center only |
| QuickML | Not available in JP, SA, CA |

## how the wiki works

The LLM follows the operating schema in [`AGENTS.md`](AGENTS.md):

- **Ingest**: Read raw source → create source summary + entity/concept pages → update index/log/overview
- **Query**: Read index → find relevant pages → synthesize answer → optionally file as analysis
- **Lint**: Check for contradictions, orphan pages, missing entities, stale content

Every page has YAML frontmatter, `[[wikilinks]]` for cross-references, and `[Source: filename]` citations. Raw sources are **never modified**.

## known gaps

- NoSQL SDK pages failed to load — may not exist in Java SDK v1
- Node.js SDK and other language SDKs not yet covered
- Security Rules and API Gateway not yet documented
- Platform pricing, limits, and deployment mechanics unknown
- Roles ↔ scopes mapping needs deeper analysis
- Connectors vs Connections SDK disambiguation incomplete

## contributing

1. Drop a new source document in `raw/`
2. Tell the LLM to ingest it
3. The LLM creates/updates wiki pages, index, log, and overview automatically

Or ask the LLM a question — if the answer is substantive, it gets filed back into `wiki/analyses/`.

## license

Knowledge base structure and content. Not affiliated with Zoho Corporation. Catalyst SDK documentation is property of Zoho.
