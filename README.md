# MCP servers that are measured, not just built.

I'm a solo developer in Türkiye. I turn your company's API into a tested MCP (Model Context Protocol) server that Claude, Cursor, VS Code and, with an OAuth add-on, ChatGPT can use. The price is fixed, delivery takes 5 business days, and you get an eval report on 10 realistic tasks plus a compatibility table. I work async, in writing.

## MCP Server Sprint

- Up to 8 tools designed for AI agents, not a 1:1 copy of your endpoints
- Built on the current MCP spec (2026-07-28) and the official TypeScript SDK v2
- Deployed to Cloudflare Workers or to your own infrastructure
- Tests, plus MCP Inspector and conformance checks
- Setup guides for Claude Code, Cursor, VS Code and Claude Desktop
- Optional OAuth 2.1 add-on for claude.ai and ChatGPT connectors
- 14 days of fixes included

If the acceptance tasks we agree on don't work, you don't pay.

For servers you already run, there is **MCP Audit & Upgrade**: SSE → Streamable HTTP, SDK v2, OAuth with CIMD (Client ID Metadata Documents).

**Live demo:** US electricity tariffs inside Claude: `https://mcp.wattclear.com/mcp` ([code, tests and eval](https://github.com/bulletrainqe/wattclear-mcp))

## Case studies

### Kalkan: payout audit for Trendyol sellers

**Problem.** Sellers on Trendyol, a Turkish marketplace, need to check that each payout is correct and to find where money leaks out.

**What I built.** A payout-audit MCP server with 7 read-only tools. Of these, 3 work (`seller_brief`, `explain_payout`, `find_leaks`) and 4 are in progress. Money is stored as bigint minor units, with one rounding function. Buyer personal data is dropped at ingestion, and marketplace credentials are encrypted with AES-256-GCM.

**Proof.** 152 tests (Vitest and fast-check property tests) and a 100% coverage gate on the core. Evals: payouts reconcile to the cent; each of the 10 leak rules must reach precision ≥ 0.90 and recall ≥ 0.80; `tools/list` is kept ≤ 3,000 tokens.

**Stack.** TypeScript (strict, ESM), Hono, Drizzle (PGlite in dev), Zod 4, and the official MCP TypeScript SDK over stateless Streamable HTTP.

**Status.** In development and fixture-tested. No live API calls yet.

### wattclear: US electricity tariffs

**Problem.** US electricity prices differ by utility and by plan, and the details sit in technical tariff documents. Even a simple question, such as what an appliance costs to run, starts with finding the right utility and tariff.

**What I built.** A tariff engine that turns tariff rules into dollar amounts, and a website on top of it. The website offers a ZIP code to utility lookup, bill analysis, plan comparison and appliance running costs.

**Proof.** 1,211 quotable US tariffs, sourced from OpenEI's Utility Rate Database (URDB, released under CC0), plus data from the U.S. Energy Information Administration (EIA). The project has 1,107 automated tests.

**Stack.** Next.js 16 on Cloudflare Workers via OpenNext.

**Status.** Live. Now also available as a public MCP server: [wattclear-mcp](https://github.com/bulletrainqe/wattclear-mcp)

### DueRiva: expiry reminders on Android

**Problem.** Documents, insurance policies and vehicle inspections each expire on their own date, often months or years apart. Keeping track of all of them by hand is easy to get wrong.

**What I built.** An Android app that reminds you before any of them expires. Documents can be captured with ML Kit's document scanner, and ML Kit OCR reads their text. Data is encrypted with Android Keystore (AES-GCM). The interface is built with Jetpack Compose, local storage uses Room, and background work runs on WorkManager.

**Proof.** 196 tests: 160 JVM unit tests and 36 instrumented tests that run on a device or emulator.

**Stack.** Kotlin 2.3, Jetpack Compose, Room, WorkManager, ML Kit, Android Keystore.

**Status.** Working prototype. A closed test on Google Play is the next step.

Also: **Codotive**, an MCP server that finds the reasons behind code decisions in a repo's history. Node.js, SQLite, official MCP SDK v2, stdio, 38 tests.

## Work with me

I work through Upwork, where the MCP Server Sprint is available as a fixed-price project: [my Upwork profile](https://www.upwork.com/freelancers/~01800cc6ed7fb14088). Tell me which API you want to expose and what an agent should be able to do with it, and I will reply in writing.
