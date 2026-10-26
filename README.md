# sllmwiki-demo

A Structural LLM Wiki: this team's knowledge, kept in git by a coding agent with the sllmwiki skill (<https://github.com/mengjiasun1992/structural-llm-wiki>), and reviewed by people like code.

- **Ask it:** `/sllmwiki:query "How does the gateway retry failed requests?"` answers from what the wiki already knows, citing it.
- **Grow it:** `/sllmwiki:build "How does the gateway route requests?"` finds sources, writes an answer whose every claim is cited, and opens one pull request for you to review and merge.
- **Browse it:** [SLLMWIKI_INDICES.md](sllmwiki/.SLLMWIKI_INDICES.md) maps the topics; each opens a page of its narratives, sources and who to ask.
- **Read it as a book:** `/sllmwiki:release` publishes a release with the wiki as a book, in the formats `Config.toml`'s `[structural-llm-wiki-release]` turns on; the latest is on this repository's [*Releases* page](https://github.com/mengjiasun1992/sllmwiki-demo/releases/latest).

## What this demo shows

This wiki is the sllmwiki skill's example, on distributed systems and moving averages. Each narrative answers one question, in the template its subject calls for:

| Question | Template |
|---|---|
| [What is a distributed system?](sllmwiki/narratives/what-is-a-distributed-system.md) | `concept_brief_overview` |
| [What are the common design tenets and principles for distributed systems?](sllmwiki/narratives/what-are-the-common-design-tenets-and-principles-for-distributed-systems.md) | `survey` |
| [What is an EWMA (exponentially weighted moving average), and how does it work?](sllmwiki/narratives/what-is-an-ewma-exponentially-weighted-moving-average-and-how-does-it-work.md) | `technical_brief_overview` |
| [When should software use an EWMA instead of other moving averages?](sllmwiki/narratives/when-should-software-use-an-ewma-instead-of-other-moving-averages.md) | `comparison` |
| [What does "Keeping CALM: When Distributed Consistency is Easy" argue, and is it worth reading in full?](sllmwiki/narratives/what-does-keeping-calm-argue-and-is-it-worth-reading-in-full.md) | `skim_read`, this wiki's own |

- **A template of its own:** [`sllmwiki/.templates/skim_read/`](sllmwiki/.templates/skim_read) is a ten-minute primer on one long work (a paper, an article, chapters of a book) for a reader from the field deciding whether to read it in full. It was added in [PR #14](https://github.com/mengjiasun1992/sllmwiki-demo/pull/14).
- **A run's pull request:** [PR #6](https://github.com/mengjiasun1992/sllmwiki-demo/pull/6) wrote four of the narratives in one unattended run: its checklist, the decisions it left, its sources and what it took.
- **The book:** each [release](https://github.com/mengjiasun1992/sllmwiki-demo/releases/latest) attaches the whole wiki as a PDF, an EPUB and one HTML file.

## Sources in a public wiki

This demo is meant to be public, so it follows the skill's source policy for a public wiki ([Licenses, sources and your responsibility](https://github.com/mengjiasun1992/structural-llm-wiki#licenses-sources-and-your-responsibility)):

- **A copy only of an openly licensed source.** A source is saved verbatim in `sllmwiki/sources/` only when its license lets anyone share it: CC0 and public-domain marks, CC BY and CC BY-SA, MIT, Apache 2.0, BSD, ISC, MPL 2.0 and the Unlicense.
- **A link for everything else.** Any other source is recorded as a link: its URL, license, tags and abstract, with no copy. A narrative can't cite a link by section, so every claim cites a saved, openly licensed source.
- **Checked on every pull request.** `wiki-check`, in the wiki's CI, fails on a copy a public wiki may not hold.

Today the wiki keeps **23 copies** (12 under CC BY-SA 4.0, 8 under CC BY 4.0 and 3 under Apache 2.0) and **22 links**:

- 12 proprietary sources, such as Hamilton's internet-scale services paper, the AWS Builders' Library, Brewer, Raft, Lamport, Saltzer and the DSP Guide;
- 4 with no stated license;
- the Google SRE book chapter, under CC BY-NC-ND;
- 5 whose terms may allow copies but that aren't on the policy's open list: three NIST pages (US-government public domain), RFC 6298 (IETF Trust) and the Linux kernel's `average.h` (GPL-2.0).

A private wiki may keep reference copies of most sources. This one started that way and was moved to openly licensed sources in [PR #16](https://github.com/mengjiasun1992/sllmwiki-demo/pull/16) and [PR #17](https://github.com/mengjiasun1992/sllmwiki-demo/pull/17), which also re-answered its narratives from them.

## What's here

| Folder | What it holds |
|---|---|
| `sllmwiki/sources/` | Sources, saved verbatim with their URL and license; `manifest.yaml` lists them |
| `sllmwiki/narratives/` | One narrative per question; `manifest.yaml` records their tags and the source versions they read |
| `sllmwiki/tags/` | The tag tree, `tags.yaml`, and a generated page per tag |
| `sllmwiki/.templates/` | This wiki's own narrative templates: `skim_read` |
| `sllmwiki/Config.toml` | This wiki's settings |

## Reading a narrative

Every claim cites the section of a source it rests on: the number links to it. Citations sit inside the sentence, before its period, as in a paper. What no source says is marked where a citation would go, `[→]` (the narrative's own reasoning) or `[?]` (something the sources don't answer), or once on a heading for a whole section; a narrative defines the ones it uses. When `Config.toml` sets `markers = false`, there are no markers: an uncited sentence is the narrative's own.

## Joining

Clone this repository and start Claude Code in it. Trust the folder, and accept the `sllmwiki` plugin it offers.
