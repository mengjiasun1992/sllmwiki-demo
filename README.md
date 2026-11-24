# sllmwiki-demo

A Structural LLM Wiki: this team's knowledge, kept in git by a coding agent with the sllmwiki skill (<https://github.com/mengjiasun1992/structural-llm-wiki>), and reviewed by people like code. It runs on sllmwiki 1.4.0.

- **Ask it:** `/sllmwiki:query "How does the gateway retry failed requests?"` answers from what the wiki already knows, citing it.
- **Grow it:** `/sllmwiki:build "How does the gateway route requests?"` finds sources, writes an answer whose every claim is cited, and opens one pull request for you to review and merge.
- **Browse it:** [SLLMWIKI_INDICES.md](sllmwiki/.SLLMWIKI_INDICES.md) maps the topics; each opens a page of its narratives, sources and who to ask.
- **Read it as a book:** `/sllmwiki:release` publishes a release with the wiki as a book, in the formats `Config.toml`'s `[structural-llm-wiki-release]` turns on; the latest is on this repository's [*Releases* page](https://github.com/mengjiasun1992/sllmwiki-demo/releases/latest).

## What this demo shows

This wiki is the sllmwiki skill's example. It covers three fields: distributed systems, with consensus and failure modes; moving averages; and machine learning, on transformer attention. Each narrative answers one question, in the template its subject calls for:

| Question | Template |
|---|---|
| [What is a distributed system?](sllmwiki/narratives/what-is-a-distributed-system.md) | `concept_brief_overview` |
| [What are the common design tenets and principles for distributed systems?](sllmwiki/narratives/what-are-the-common-design-tenets-and-principles-for-distributed-systems.md) | `survey` |
| [Can you walk me through the classic failure modes in large scale distributed system and what are the common solutions to prevent or mitigate each one?](sllmwiki/narratives/what-are-the-classic-failure-modes-of-large-scale-distributed-systems-and-their-mitigations.md) | `survey` |
| [How does Raft elect a leader and keep every server's log consistent through crashes and network partitions?](sllmwiki/narratives/how-does-raft-elect-a-leader-and-keep-every-servers-log-consistent.md) | `technical_brief_overview` |
| [What does "Keeping CALM: When Distributed Consistency is Easy" argue, and is it worth reading in full?](sllmwiki/narratives/what-does-keeping-calm-argue-and-is-it-worth-reading-in-full.md) | `skim_read`, this wiki's own |
| [What is an EWMA (exponentially weighted moving average), and how does it work?](sllmwiki/narratives/what-is-an-ewma-exponentially-weighted-moving-average-and-how-does-it-work.md) | `technical_brief_overview` |
| [When should software use an EWMA instead of other moving averages?](sllmwiki/narratives/when-should-software-use-an-ewma-instead-of-other-moving-averages.md) | `comparison` |
| [Can you compare the classic attention mechanism and paged attention mechanism for transformer model architecture?](sllmwiki/narratives/how-does-classic-attention-compare-with-paged-attention-in-transformers.md) | `comparison` |

- **Written to sllmwiki 1.4.0's guides:** every narrative was regenerated from a new outline in [PR #30](https://github.com/mengjiasun1992/sllmwiki-demo/pull/30) and [PR #31](https://github.com/mengjiasun1992/sllmwiki-demo/pull/31). Each one follows a single running example, set up once. Where three or more ideas are new, it builds them up on a toy small enough to compute by hand before the real case. Every number shows its arithmetic.
- **Pictures:** mermaid diagrams for structure and flow, and SVG charts for curves and measured values. Each chart is drawn from the narrative's own cited numbers, which its `<desc>` keeps. They live in [`sllmwiki/narratives/figures/`](sllmwiki/narratives/figures).
- **Checked by an independent lint:** each run's lint step had a separate reviewer check every claim against the section it cites, recompute every number, and report what to fix. Each pull request's description lists the findings, what was fixed, and what was left for the owner.
- **A template of its own:** [`sllmwiki/.templates/skim_read/`](sllmwiki/.templates/skim_read) is a ten-minute primer on one long work (a paper, an article, chapters of a book) for a reader from the field deciding whether to read it in full. It was added in [PR #14](https://github.com/mengjiasun1992/sllmwiki-demo/pull/14).
- **A run's pull request:** [PR #24](https://github.com/mengjiasun1992/sllmwiki-demo/pull/24) researched a new field in one unattended run: its tags, its sources, the decisions it left, and its lint.
- **The book:** each [release](https://github.com/mengjiasun1992/sllmwiki-demo/releases/latest) attaches the whole wiki as a PDF, an EPUB and one HTML file.

## Sources in a public wiki

This demo is meant to be public, so it follows the skill's source policy for a public wiki ([Licenses, sources and your responsibility](https://github.com/mengjiasun1992/structural-llm-wiki#licenses-sources-and-your-responsibility)):

- **A copy only of an openly licensed source.** A source is saved verbatim in `sllmwiki/sources/` only when its license lets anyone share it: CC0 and public-domain marks, CC BY and CC BY-SA, MIT, Apache 2.0, BSD, ISC, MPL 2.0 and the Unlicense.
- **A link for everything else.** Any other source is recorded as a link: its URL, license, tags and abstract, with no copy. A narrative can't cite a link by section, so every claim cites a saved, openly licensed source.
- **Checked on every pull request.** `wiki-check`, in the wiki's CI, fails on a copy a public wiki may not hold.

Today the wiki keeps **34 copies** (16 under CC BY-SA 4.0, 13 under CC BY 4.0 and 5 under Apache 2.0) and **25 links**:

- 14 proprietary sources, such as the Raft paper, Brewer, Lamport, Saltzer, Hamilton's internet-scale services paper, the AWS Builders' Library, the DSP Guide, and the metastable-failure and gray-failure papers;
- 5 with no stated license, among them the original attention paper, whose arXiv license allows distribution by arXiv only;
- the Google SRE book chapter, under CC BY-NC-ND;
- 5 whose terms may allow copies but that aren't on the policy's open list: three NIST pages (US-government public domain), RFC 6298 (IETF Trust) and the Linux kernel's `average.h` (GPL-2.0).

A private wiki may keep reference copies of most sources. This one started that way and was moved to openly licensed sources in [PR #16](https://github.com/mengjiasun1992/sllmwiki-demo/pull/16) and [PR #17](https://github.com/mengjiasun1992/sllmwiki-demo/pull/17), which also re-answered its narratives from them.

## What's here

| Folder | What it holds |
|---|---|
| `sllmwiki/sources/` | Sources, saved verbatim with their URL and license; `manifest.yaml` lists them |
| `sllmwiki/narratives/` | One narrative per question; `manifest.yaml` records their tags and the source versions they read; `figures/` holds their SVG charts |
| `sllmwiki/tags/` | The tag tree, `tags.yaml`, and a generated page per tag |
| `sllmwiki/.templates/` | This wiki's own narrative template, `skim_read`, and `audience.md`, who the wiki is written for (empty: the skill's default reader) |
| `sllmwiki/Config.toml` | This wiki's settings, including each template's word budget and the picture formats allowed (mermaid, SVG and PNG, at most 5 a narrative, 100 KB an image) |

## Reading a narrative

Every claim cites the section of a source it rests on: the number links to it. Citations sit inside the sentence, before its period, as in a paper. What no source says is marked where a citation would go, `[→]` (the narrative's own reasoning) or `[?]` (something the sources don't answer), or once on a heading for a whole section; a narrative defines the ones it uses. When `Config.toml` sets `markers = false`, there are no markers: an uncited sentence is the narrative's own.

## Joining

Clone this repository and start Claude Code in it. Trust the folder, and accept the `sllmwiki` plugin it offers. The skill's commands come from the `sllmwiki` package, version 1.4.0, which the plugin installs on first use.
