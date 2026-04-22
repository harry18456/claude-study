# article-verification Specification

## Purpose

TBD - created by syncing change 'verify-articles-2026-04'. Update Purpose after archive.

## Requirements

### Requirement: Produce one verification report per article

The system SHALL produce exactly one verification report file for each of the nine source articles. Each report MUST be written to `verify/<filename>.md`, where `<filename>` matches the source article filename (e.g., `verify/01_GENERAL.md` for `01_GENERAL.md`). The source article files MUST NOT be modified as part of generating these reports.

#### Scenario: Creating a report for a source article

- **WHEN** the verifier processes `01_GENERAL.md`
- **THEN** the system SHALL create `verify/01_GENERAL.md` and SHALL NOT write to `01_GENERAL.md`

#### Scenario: All nine articles covered

- **WHEN** the verification workflow completes successfully
- **THEN** the `verify/` directory SHALL contain exactly these nine files: `01_GENERAL.md`, `02_CHAT.md`, `03_COWORK.md`, `04_CODE.md`, `05_API.md`, `06_MCP.md`, `07_SKILL.md`, `08_MANAGED_AGENTS.md`, `appendix_claude_design_prompt.md`


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Report structure

Each verification report MUST contain three sections in this order: a Header section, a Claims Table, and a Summary section. The Header MUST state the source filename, source line count, count of external URLs in the source, verification date in `YYYY-MM-DD` format, and the verifier identity (model name and version). The Claims Table MUST use these columns in this order: `#`, `Line`, `Claim (excerpt)`, `Type`, `Result`, `Source / Suggestion`. The Summary MUST state counts for each of the four result markers, list the three to five most critical corrections, and give an overall risk assessment of `Low`, `Medium`, or `High`.

#### Scenario: Valid report header

- **WHEN** a verification report is generated
- **THEN** the Header section SHALL contain filename, line count, URL count, verification date (YYYY-MM-DD), and verifier identity

#### Scenario: Claims table columns

- **WHEN** a claim row is added to the Claims Table
- **THEN** it SHALL have values for all six columns: `#`, `Line`, `Claim (excerpt)`, `Type`, `Result`, `Source / Suggestion`

#### Scenario: Summary counts and risk level

- **WHEN** the Summary section is written
- **THEN** it SHALL include counts for ✅, ⚠️, ❌, and 🔍 markers, the top 3–5 critical corrections, and an overall risk label from {Low, Medium, High}


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Four-class result markers

Each claim entered in the Claims Table MUST be assigned exactly one of four result markers: `✅ Verified`, `⚠️ Needs correction`, `❌ Unsupported`, or `🔍 Unverifiable`. The `Source / Suggestion` column MUST contain the content mandated by that marker:

- `✅ Verified` MUST include a source URL. If the source article does not already cite that URL, the suggestion MUST state "suggest adding citation".
- `⚠️ Needs correction` MUST name the specific error (wrong number, date, version, name) and MUST provide the corrected value with a source URL.
- `❌ Unsupported` MUST list which source categories were searched and MUST recommend one of: delete, rewrite as non-assertive, or retain with a pending-source note.
- `🔍 Unverifiable` MUST state why verification is impossible (subjective, non-public, internal knowledge, etc.).

#### Scenario: Verified claim includes a URL

- **WHEN** a claim is marked `✅ Verified`
- **THEN** the `Source / Suggestion` cell SHALL contain at least one URL

#### Scenario: Needs-correction claim names the error and correction

- **WHEN** a claim is marked `⚠️ Needs correction`
- **THEN** the `Source / Suggestion` cell SHALL identify the specific incorrect value and the corrected value with a URL

#### Scenario: Unsupported claim documents search effort

- **WHEN** a claim is marked `❌ Unsupported`
- **THEN** the `Source / Suggestion` cell SHALL list the source tiers searched and propose one of: delete, rewrite, or retain with a note

#### Scenario: Unverifiable claim justifies non-verification

- **WHEN** a claim is marked `🔍 Unverifiable`
- **THEN** the `Source / Suggestion` cell SHALL state the reason verification is not feasible


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Source credibility hierarchy

When verifying a claim, the verifier SHALL attempt sources in this priority order and SHALL prefer higher-tier sources when multiple are available:

1. Primary official sources: `anthropic.com`, `claude.com`, `code.claude.com`, `platform.claude.com`, `modelcontextprotocol.io`, official Anthropic GitHub repositories, the official Anthropic blog and Engineering blog, and the official Anthropic YouTube channel.
2. Official auxiliary documentation: `support.claude.com`, official changelogs, and official API references.
3. First-tier media reporting: TechCrunch, The Verge, The Register, iThome, TheNewStack, Bloomberg, WSJ — acceptable only when directly quoting Anthropic or reporting on an Anthropic-issued benchmark.
4. Community and personal sources: Hacker News threads, well-known personal blogs — acceptable ONLY as supplementary evidence, never as the sole source.

Tweets, Reddit posts, and anonymous forum posts MUST NOT be cited unless they link directly to a first-tier source.

#### Scenario: Official and media sources conflict

- **WHEN** an official source and a media source report different values for the same claim
- **THEN** the verifier SHALL record the value from the official source and SHALL note the discrepancy in the `Source / Suggestion` cell

#### Scenario: Only a community source is found

- **WHEN** the only available evidence is a Hacker News post or personal blog
- **THEN** the claim SHALL NOT be marked `✅ Verified` on that evidence alone


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Do not modify source articles

The verification workflow MUST NOT modify any file whose name matches `0[1-8]_*.md` or `appendix_*.md` in the repository root. Corrections are produced as suggestions in reports only; the decision to apply, rewrite, or delete text is left to a separate, subsequent change.

#### Scenario: Verifier attempts to edit source article

- **WHEN** the verifier encounters an unsupported claim
- **THEN** the verifier SHALL record the suggestion in the report and SHALL NOT edit the source article


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Processing order

The verifier MUST process articles in this fixed order: `01_GENERAL.md`, `07_SKILL.md`, `02_CHAT.md`, `06_MCP.md`, `03_COWORK.md`, `05_API.md`, `08_MANAGED_AGENTS.md`, `appendix_claude_design_prompt.md`, `04_CODE.md`. Each report MUST be completed and saved before the next article begins.

#### Scenario: Starting verification

- **WHEN** the verification workflow begins with no existing reports
- **THEN** the first report created SHALL be `verify/01_GENERAL.md`

#### Scenario: Resuming after interruption

- **WHEN** the workflow resumes after some reports are complete
- **THEN** the verifier SHALL continue from the next article in the fixed order, not from an arbitrary article


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Claim detection scope

A "hard claim" — which the verifier MUST attempt to verify — is any statement that asserts a specific date, version number, benchmark figure, price, URL, event timeline, named product feature, named case study, or cited quotation. Pure operational instructions (numbered steps, button labels), subjective recommendations, and hedged statements using "approximately", "typically", or similar qualifiers are NOT hard claims and MUST NOT be entered in the Claims Table.

#### Scenario: A version number appears in the source

- **WHEN** the source states "v2.1.113 introduced native binaries"
- **THEN** the verifier SHALL enter this as a claim of type "version number"

#### Scenario: A numbered operational step appears

- **WHEN** the source states "1. Open Settings 2. Click Capabilities"
- **THEN** the verifier SHALL NOT enter this as a claim


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Web verification method

For each hard claim, the verifier SHALL use `WebSearch` or `WebFetch` to locate evidence. For existing URLs in the source article, the verifier SHALL call `WebFetch` to confirm reachability and topical relevance; an HTTP error response alone MUST NOT be treated as evidence of fabrication, and the verifier SHALL distinguish transient server errors from permanent 404s and SHALL consult an archive snapshot when a permanent failure is suspected.

#### Scenario: Verifying an existing URL

- **WHEN** a source article cites `https://www.anthropic.com/engineering/managed-agents`
- **THEN** the verifier SHALL fetch the URL and confirm the page exists and discusses Managed Agents

#### Scenario: URL returns 500

- **WHEN** `WebFetch` returns an HTTP 500 for a cited URL
- **THEN** the claim SHALL NOT be marked `❌ Unsupported` on that basis alone; the verifier SHALL retry or check an archive snapshot


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->

---
### Requirement: Verifier self-bias mitigation

The verifier MUST NOT mark a claim `✅ Verified` on the sole basis that the claim appears plausible to the verifier's own model. A verifiable source URL MUST be recorded for every `✅ Verified` entry.

#### Scenario: Plausibility without evidence

- **WHEN** the verifier cannot locate external evidence for a claim but judges it plausible
- **THEN** the claim SHALL be marked `❌ Unsupported` or `🔍 Unverifiable`, not `✅ Verified`


<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
-->

<!-- @trace
source: verify-articles-2026-04
updated: 2026-04-22
code:
  - verify/04_CODE.md
  - verify/README.md
  - verify/03_COWORK.md
  - verify/appendix_claude_design_prompt.md
  - verify/SUMMARY.md
  - verify/07_SKILL.md
  - verify/06_MCP.md
  - verify/02_CHAT.md
  - verify/08_MANAGED_AGENTS.md
  - verify/05_API.md
  - verify/_template.md
  - CLAUDE.md
  - verify/01_GENERAL.md
-->