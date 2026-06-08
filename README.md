# Leonardo Maçan — Builder Profile

Digital savvy Product Manager. Brazilian & Italian. Based in São Paulo - Brazil.

I move between strategy and execution — I define roadmaps, prioritize with data, and also write the tools that make my team faster. Most of what I've built starts the same way: I hit a friction point, realize there's no good solution, and go build one.

[linkedin.com/in/lmacan](https://www.linkedin.com/in/lmacan) · macanleonardo@gmail.com

---

## What I'm currently working on 

### Product Observer
*Map any web app's API without touching its code*

The insight: PMs need to understand how a product works at the API layer — for discovery, for onboarding, for writing better specs — but most don't read code. The usual answer is "ask engineering." I wanted a better answer.

Product Observer opens a real browser, you use the app normally, and it captures every API call in the background. Then it clusters endpoints by domain, infers response schemas, and produces a plain-markdown API catalog and workflow map — no backend access, no code, no API keys required.

It runs entirely inside Claude Code using a custom skill. Four automated phases: capture → cluster → annotate → document. The annotation layer learns from every run and gets smarter over time, building a `knowledge/` store per target system.

**Ideas behind it:**
- Observing is safer and more honest than reverse-engineering. The tool is strictly read-only — it never replays or modifies requests.
- PMs shouldn't need to rely on engineers to understand their own product's surface area.
- The output (plain markdown) should open in any editor, not require a specific tool.

**Stack:** Python, Playwright (Chromium), Pydantic, Claude Code skills.

---

### Regulatory Radar
*Segmented alerts from Brazilian official gazettes*

Brazil publishes government acts across thousands of municipal and federal official gazettes (Diários Oficiais). No one reads them. Buried in the noise: contract awards, regulatory changes, personnel acts — things that matter to legal teams, public affairs, and mid-size companies.

The idea is a radar that ingests these gazettes daily and delivers segmented alerts by customer profile: sector, jurisdiction, act type. Think of it as a newsfeed for what government actually does, curated per subscriber.

I ran the data discovery myself before writing a line of product spec — queried the Querido Diário API, mapped coverage across all 5,570 Brazilian municipalities, validated that text extraction is already done (no PDF parsing needed in the pipeline), and compared the municipal pipeline against the federal DOU (Inlabs) structure.

**MVP scoping decisions:**
- **Alagoas as the technical lab:** 92 municipalities entered via a single state consortium on the same date — standardized format, predictable pipeline, low parsing variance.
- **São Paulo as the market entry:** 112 municipalities, highest commercial density, target customers (law firms, consulting, corporate legal) are concentrated here.
- **Federal DOU (Inlabs) deferred to v2:** access already obtained, XML structure mapped, but running both pipelines in parallel in the MVP risks focus fragmentation.

**Stack (planned):** Python, Querido Diário API, Inlabs (DOU federal), LLM for relevance classification.

---

### mcp-playwright-scraper
*An MCP server that gives Claude a real browser*

Claude can't read the web out of the box. This is a Model Context Protocol server that wraps Playwright + BeautifulSoup + Pandoc into a single `scrape_to_markdown` tool — you give it a URL, it returns clean Markdown.

The practical use case: pulling live documentation, product pages, or any JavaScript-heavy site directly into a Claude session without leaving the chat. Works with Claude Desktop, Claude Code, Cursor, and Zed.

Published to PyPI. Installable via `pip` or `uvx`.

**Stack:** Python, Playwright (Chromium), BeautifulSoup, Pypandoc, MCP SDK.

---

### Product Pilot *(Playwright automation agent)*
*Headless automation for authenticated corporate flows*

The problem: repetitive daily workflows locked behind VPN + 2FA — report exports, data pulls, form submissions. Doing them manually wastes time. Letting an LLM drive the browser on every run is overkill and adds latency.

The architecture I settled on is deliberately simple: record once (Playwright codegen, manual session), convert the raw output to a semantic `flow-map.json`, then execute headlessly every day — zero LLM in the loop. Claude only enters if the output needs interpretation.

Authentication is handled via Playwright's native `storageState` — authenticate once with the browser open, save `session.json`, all future runs load the session and skip the login.

**Key design decision:** deterministic execution beats agentic execution when the flow is fixed. LLMs are expensive and slow for tasks that don't require reasoning.

**Stack:** Node.js, Playwright, JSON flow maps, `storageState` for session persistence.

---

### Combat Coach *(personal project)*
*AI coach for combat sports*

The gap: combat sports knowledge lives in PDFs, YouTube videos, and coaches' heads — none of it is queryable. I wanted something I could ask "what are the entries for a high crotch from the Russian tie?" and get a grounded, source-linked answer.

The architecture is a multi-agent RAG system: documents ingested into a vector store (ChromaDB), BM25 hybrid retrieval, a LangGraph agent graph managing conversation state, and a Chainlit frontend for the chat interface. Local inference via Ollama for cost-free iteration, Anthropic for production quality.

**Stack:** Python, LangGraph, ChromaDB, Chainlit, Anthropic SDK, Ollama, PyMuPDF, BM25.

---

### FRF Editor *(Feature Request Form auditor)*
*AI review of product intake specs against quality guidelines*

Feature Request Forms — the intake artifact before engineering picks up work — are inconsistently written across teams. Wrong level of detail, missing acceptance criteria, unclear operational impact.

The tool audits FRFs against a company template and quality guidelines using an LLM. The product has a full PRD, story specs, and a PRD ↔ Stories traceability matrix. Currently in the discovery and planning phase — scaffolding next.

**Stack (planned):** Python, Anthropic SDK, structured LLM output (JSON), Cursor-guided agent workflow.

---

## Other tools (internal, at Shopee)

- **wms-putaway-audit** — Automated Putaway metrics audit, pulls warehouse data and generates structured team reports. Built after identifying that a 100%+ lead time inflation was coming from tasks that crossed day boundaries, not from actual execution.
- **notion-reporter** — CLI that pulls Notion tasks, groups by status, enriches with page content, and generates LLM summaries. Replaced a manual weekly reporting ritual.
- **brd-analyzer** — AI-powered Business Requirements Document analyzer: extracts operational impact, generates ops-ready questions, syncs summaries to Obsidian.

---

## How I approach building

**I start with friction, not features.**

**I do the discovery first.** 

**I match the architecture to the problem.** 

**I build for non-technical users when it matters.** 

---
