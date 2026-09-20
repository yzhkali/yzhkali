# Deferring Dify's tool-list JavaScript until first use

Opening a workflow, knowledge pipeline, or snippet editor loaded the tool
list's pinyin dependency before a user opened the list. The proposed change
moves that work behind dynamic imports. In the measured production frontend
scenarios, each route loaded **291,444 fewer JavaScript response body bytes
initially**; opening Tools later incurred three additional requests.

[PR #42580](https://github.com/langgenius/dify/pull/42580) is **open and awaiting
review**, not merged, as of September 20, 2026. It addresses the concrete
`pinyin-pro` follow-up in [issue #42437](https://github.com/langgenius/dify/issues/42437),
not the entire dependency audit.

## Find the right loading boundary

The shared `tool-list-data.ts` helper imports `pinyin-pro` for Chinese tool
grouping. Its two runtime consumers are the normal `Tools` list and the RAG
recommendation `List`. Making just one caller asynchronous would leave other
static import paths into the editor bundle.

The change covers all five runtime list call sites: `tool-browser.tsx`,
`data-sources.tsx`, `featured-tools.tsx`, `rag-tool-recommendations/index.tsx`,
and `agent-strategy-selector.tsx`. They use the existing dynamic-import helper
and loading placeholder. The list keeps its original grouping and sorting;
the surrounding components retain search, category/view controls, popup state,
and selection callbacks.

```text
Before: editor -> list owner -> tool-list-data -> pinyin-pro
After:  editor -> controls + placeholder
                     -> dynamic list, when rendered -> tool-list-data -> pinyin-pro
```

Review the [implementation and tests at the measured revision](https://github.com/langgenius/dify/commit/0c616fc7dde1de50629e764ca9fbc3348bc8f030).

## Measure the result and first-use cost

The comparison used a production Vinext standalone build, Node 24.21.0,
pnpm 12.4.2, Vinext 1.0.0-beta.10, and Chromium 153. Each route opened in a
fresh browser context with cache disabled and a 1440 × 960 viewport.
Both revisions used identical local API fixtures: a two-node workflow/snippet,
a pipeline knowledge node, and one installed tool provider.

- Baseline: [`2590d907`](https://github.com/langgenius/dify/commit/2590d907698355ee2db0ede64d926112d8b96df1).
- Proposed change: [`0c616fc7`](https://github.com/langgenius/dify/commit/0c616fc7dde1de50629e764ca9fbc3348bc8f030).
- [Recorded measurement data](data/dify-tool-loading.json), September 20, 2026.

JavaScript response body bytes at initial network idle, before opening a selector:

| Route | Before | After | Deferred | Initial JS requests |
| --- | ---: | ---: | ---: | ---: |
| Workflow | 8,141,637 | 7,850,193 | 291,444 | 498 → 498 |
| Knowledge pipeline | 7,912,896 | 7,621,452 | 291,444 | 485 → 485 |
| Snippet | 7,842,514 | 7,551,070 | 291,444 | 476 → 476 |

The server returned JavaScript **without content encoding**. These are body
bytes, not compressed transfer sizes or a timing metric. The chunk containing
`pinyin-pro` was absent initially and requested after opening the Tools tab.

First use added **3 requests and 303,933 body bytes**. Cumulative JavaScript
after opening Tools was therefore **12,489 bytes higher than baseline**.
The benefit is deferring optional work; this does not remove the dependency
or reduce total bytes for someone who opens the tool list.

## Protect interactions during loading

Two new regression tests deliberately hold the list import unresolved:

1. Change the filter before the code arrives, then confirm the loaded list
   uses the current selection.
2. Collapse and reopen the RAG recommendations while loading, then select a
   Chinese-labeled provider after the list resolves.

Both tests failed against the eager-loading baseline and passed with the
change. The affected suite passed **171 tests across 36 files**. Static checks,
TSSLint, and both Vinext and Next.js production builds passed.

Browser checks on all three production routes covered searching, Escape
dismissal, reopening with reset search, and inserting a tool. Holding the
actual list chunk request also exercised controls and closing/reopening while
the code was still loading. The recorded runs had no page errors or failed
HTTP responses.

## Recheck the behavior

Build and serve both revisions in production mode with identical route data
and compression settings. In a fresh browser context with cache disabled,
load each editor route and record script response body sizes at network idle.
Then open **Add Node → Tools**, wait for the list, and record the additional
requests. Confirm that the pinyin-containing chunk appears only at first use.
Delay that chunk separately to check controls before it arrives.

The linked JSON preserves the measured results; it is not a benchmark harness.
Exact totals depend on the build and fixture data. These checks used real
frontend route components with synthetic API responses. They did not measure
LCP or establish an end-user speed increase, and did not exercise full backend
execution, history restoration, or collaborative editing.
