# yzhkali

**AI tooling · Python & TypeScript · Open source**

I contribute fixes, tests, and performance improvements to open-source tools, with a current focus on AI applications and developer workflows.

关注 AI 应用与开发工具，通过可复现的问题、回归测试和持续贡献改进软件。

## Selected merged contributions

| Project | Contribution | Pull request |
| --- | --- | --- |
| **Dify** | Migrate education-verification storage to shared React hooks and update behavior tests. | [#36934](https://github.com/langgenius/dify/pull/36934) |
| **Dify** | Add explicit Python types for application configuration and a vector-database response helper. | [#37422](https://github.com/langgenius/dify/pull/37422) |
| **cattrs** | Support PEP 695 type aliases in union passthrough, with Python 3.12 regression coverage. | [#753](https://github.com/python-attrs/cattrs/pull/753) |
| **ran** | Preserve zero-valued root-bracketing boundaries on non-convergence; add regression tests. | [#604](https://github.com/synesenom/ran/pull/604) |
| **SentrySearch** | Clarify CPU fallback on Intel Macs and test platform-specific diagnostics. | [#75](https://github.com/ssrajadh/sentrysearch/pull/75) |

[Browse merged pull requests →](https://github.com/search?q=author%3Ayzhkali+is%3Apr+is%3Amerged&type=pullrequests)

## Current contributions

Both PRs below are **open, not yet merged**, as of September 20, 2026. Follow the links for their latest status.

- **[Dify #42580](https://github.com/langgenius/dify/pull/42580)** — Defer tool lists and pinyin grouping until needed. Includes loading-state regression tests and before/after measurements on production frontend builds.
- **[FreeLLMAPI #1276](https://github.com/tashfeenahmed/freellmapi/pull/1276)** — Add CLI provider-key management with dashboard authentication, hidden credential input, explicit failure handling, and integration tests against the real API routes.

## Project

### [ARC-AGI-3 Evolution](https://github.com/yzhkali/arc-agi3-evolution)

A Python workflow for **source-guided** environment exploration and replay: record competing hypotheses, run probes, validate action ledgers, and replay candidate plans. The repository documents the method, provenance, and run records.

## How I work

- Reproduce the problem and identify the smallest useful change.
- Add regression coverage for the behavior that matters.
- Check real interactions and packaged output where relevant.
- Document results, limitations, and review feedback.

English / 中文
