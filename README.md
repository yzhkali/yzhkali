# yzhkali

**AI tooling · Python & TypeScript · Open source**

I contribute fixes, tests, and performance improvements to open-source tools, with a current focus on AI applications and developer workflows.

关注 AI 应用与开发工具，通过可复现的问题、回归测试和持续贡献改进软件。

## Selected merged contributions

| Project | Contribution | Pull request |
| --- | --- | --- |
| **cattrs** | Support PEP 695 type aliases in union passthrough, with Python 3.12 regression coverage. | [#753](https://github.com/python-attrs/cattrs/pull/753) |
| **ran** | Preserve zero-valued root-bracketing boundaries on non-convergence; add regression tests. | [#604](https://github.com/synesenom/ran/pull/604) |
| **Dify** | Migrate education-verification storage to shared React hooks and update behavior tests. | [#36934](https://github.com/langgenius/dify/pull/36934) |
| **Dify** | Add explicit Python types for application configuration and a vector-database response helper. | [#37422](https://github.com/langgenius/dify/pull/37422) |
| **SentrySearch** | Clarify CPU fallback on Intel Macs and test platform-specific diagnostics. | [#75](https://github.com/ssrajadh/sentrysearch/pull/75) |

[Browse merged pull requests →](https://github.com/search?q=author%3Ayzhkali+is%3Apr+is%3Amerged&type=pullrequests)

## Current contributions

Both PRs below are **open, not yet merged**, as of September 20, 2026. Follow the links for their latest status.

- **[Dify #42580](https://github.com/langgenius/dify/pull/42580)** — Defer tool lists and pinyin grouping until needed. Includes loading-state regression tests and before/after measurements on production frontend builds. [Read the case study](case-studies/dify-tool-loading.md).
- **[FreeLLMAPI #1276](https://github.com/tashfeenahmed/freellmapi/pull/1276)** — Add CLI provider-key management with dashboard authentication, hidden credential input, explicit failure handling, and integration tests against the real API routes.

## Project

### [ARC-AGI-3 Evolution](https://github.com/yzhkali/arc-agi3-evolution)

A Python workflow for **source-guided** environment exploration and replay: record competing hypotheses, run probes, validate action ledgers, and replay candidate plans.

[Run the offline demo](https://github.com/yzhkali/arc-agi3-evolution#offline-quick-start) · [Walk through the evidence](https://github.com/yzhkali/arc-agi3-evolution/blob/main/docs/offline-demo.md) · [CI](https://github.com/yzhkali/arc-agi3-evolution/actions/workflows/core-tests.yml)

The standard-library demo runs a toy puzzle from probe to fresh replay, emits a JSON evidence record, and reports failure when the search budget is insufficient. Core CI covers Python 3.10, 3.12, and 3.14. The demo is separate from the documented official ARC runs.

## Engineering notes

**[Deferring Dify's tool-list JavaScript until first use](case-studies/dify-tool-loading.md)** — Trace a shared dependency through five import sites, protect interactions while code loads, and measure the first-use trade-off. Includes the exact revisions, regression evidence, and recorded request sizes.

English / 中文
