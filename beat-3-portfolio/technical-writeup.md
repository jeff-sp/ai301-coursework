# Picking and fixing my first open source issue

## Overview

Kaoto is an open-source visual designer for Apache Camel integrations. My first open-source contribution started with issue #3529, where the Cypress test stepPaste.cy.ts failed intermittently in CI. I traced the problem to how the node right-click context menu was rebuilt, then implemented a custom wrapper and regression tests to fix it. PR #4306 was approved and merged on November 11, 2026, closing the issue. I evaluated the issue using issue-scout, a Claude Code skill for which I wrote the search strategy and evaluation rubric.

Link to pull request: https://github.com/KaotoIO/kaoto/pull/4306

## What happened

### Picking the issue with issue-scout (2026-10-18)

issue-scout is a Claude Code skill that searches GitHub for good first issues and grades the best ones. I implemented search-strategy.md with four TypeScript queries and rubric.md with checks. The first scan on 2026-10-18 went from 388 results to 5 graded and 5 accepted. 

> Scan 1 (saved in scan-output.md): 388 unique results, 248 after the drop filters, 174 after the per-repo cap, 5 graded, 5 accepted, 0 rejected.

What was wrong with the result is that one organization took 3 of the 5 slots. The cap was 2 issues per repository and tdk-landscape owned several repos that matched "good first issue", so I fixed this by capping 2 issues per owner.

> Three of five graded issues are from one GitHub org (tdk-landscape) because the per-repository cap does not cap per owner.

Before, filter 4 in `search-strategy.md`:

```
F4 cap 2 per repository: 74 dropped, 174 left across 145 repos.
```

After:

```
Filter 4 changed from "2 per repository" to "2 per owner". Same 388 results: F4 now drops 80 (168 left, 139 repos).
```

Re-ranking the same 388 results with the new cap dropped 2 tdk-cli-core issues from the top 5 and let 2 test issues from another owner in. Kaoto #3529 was still not in the scan, and it reached me when I graded 7 picks from AI301's Issue Selection site with the issue-scout rubric. The rubric's Unclaimed check rejected 3 of 7 issues. The rubric accepted 4 issues and ranked Kaoto's #3529 issue first.

> Graded 7: 4 accepted (Kaoto #3529, openedx/frontend-app-authoring #2723, dapr/js-sdk #498, Kaoto #3528), 3 rejected, all three on Unclaimed

### The rubric said "ambitious" and I took it anyway (2026-10-24)

On 2026-10-24 I replaced the “Scope fits newcomer” check with `scope-feasible` plus a difficulty note. Issue #3529 still passed, but it now carried the label “ambitious”. That label moved it to second place, behind an easier issue. I chose it anyway because its open question was one I could ask a maintainer.

```
2. KaotoIO/kaoto#3529, flaky `stepPaste.cy.ts` (accept, ambitious)
- scope-feasible: pass. The issue names one flaky spec and three loading providers as suspects.
- Difficulty: ambitious, for two reasons:
  - The latest comment (`tplevko`, 2026-08-05) says the original failure no longer appears, likely fixed by PR #3522, and no maintainer has replied.
```

### I could not reproduce it (2026-10-25)

I ran the spec 21 times on Windows 11 and WSL2 and all 147 tests passed. I posted those results in my claim comment and asked the maintainers whether to keep investigating the three providers named in the issue or start from the cy.click() failure in the 2026-08-05 comment.

> That is 147 of 147 tests passed. The tests ran 2x to 5x slower under load, but neither the `context-menu-item-paste-as-*` signature nor the `cy.click() failed because the page updated` one appeared.

> Is the untrue ready signal in `runtime.provider.tsx`, `schemas.provider.tsx`, and `catalog.provider.tsx` still where you want this investigated? Or should I start from the `cy.click()` failure in the 2026-08-05 comment?

The failure I chose to track down from a contributor's comment on the issue (tplevko, 2026-08-05):

```
CypressError: Timed out retrying after 8050ms: `cy.click()` failed because the page updated while this command was executing. Cypress tried to locate elements based on this query:

> <li.pf-v6-c-menu__list-item>
```

### Finding the cause (2026-10-31)

I initially confirmed 3 providers already had the earlier fix. Next I followed the August comment's clue that the menu item is found and then detached. A `MutationObserver` trace under load showed that after a second right-click the old menu items stayed in the DOM for 11 ms and then no menu existed for 540 ms. PatternFly's `ContextMenu` effect below closes and reopens the menu whenever its children change.

```js
useEffect(() => {
  if (isOpen) {
    setOpen(false);
    requestAnimationFrame(() => setOpen(true));
  }
}, [children]);
```

### A test that fails on main

Since the Cypress spec never failed for me, a passing spec could not prove the fix. So I wrote a unit test that right-clicks the `route-8888` node twice and asserts the first menu's Delete item leaves the DOM. This unit test failed on `main` and passes with the wrapper.

```tsx
const route = screen.getByText('route-8888');
fireEvent.contextMenu(route);
const firstDelete = await screen.findByRole('menuitem', { name: 'Delete' });

fireEvent.contextMenu(route);

/* The items of the first menu must not stay clickable while the second menu is being built (#3529) */
expect(firstDelete).not.toBeInTheDocument();
const secondDelete = await screen.findByRole('menuitem', { name: 'Delete' });
expect(secondDelete).not.toBe(firstDelete);
```

On `main`:

```
expected document not to contain element, found <button
```

### My first fix did nothing on the second click (2026-10-31)

My first wrapper computed the key from the previous state. The "after" trace looked clean for the wrong reason. The second right-click on the same node changed nothing, and here is why. A real right-click fires two events: `mousedown`, then `contextmenu`. The `mousedown` closes the open menu by setting the state to `null`. React runs both updates in one batch, so by the time my updater ran, `current` was already `null`. That made the new id `(null?.id ?? 0) + 1`, which is 1, the same id the first menu had. React uses that id as the key, and an unchanged key tells it to keep the same `ContextMenu` instance, so the old items never left the DOM.

I changed to a `useRef` counter, which lives outside the state, so the queued close cannot reset it. I also added a fifth unit test that dispatches both events in one `act()`.

First version:

```ts
id = (current?.id ?? 0) + 1   // inside the state updater
```

Merged version:

```tsx
/** Counts every open, independently of the close that a `mousedown` may queue in the same event batch */
const openCount = useRef(0);

const onContextMenu = useCallback((event: MouseEvent) => {
  event.preventDefault();
  event.stopPropagation();
  openCount.current += 1;
  setMenu({ id: openCount.current, reference: { x: event.clientX, y: event.clientY } });
}, []);
```

### The pull request (2026-11-01)

I opened PR #4306 with 5 files, +272 / -11, one new wrapper, six new tests, and a one-line switch in the node and group components.

```diff
-export const CustomNodeWithSelection = withSelection()(withContextMenu(NodeContextMenuFn)(CustomNodeObserver));
+export const CustomNodeWithSelection = withSelection()(withNodeContextMenu(NodeContextMenuFn)(CustomNodeObserver));
```

A maintainer asked a question the next day, 2026-11-02. I did not see it for three days. I then spent the rest of the week checking every claim in my draft reply against the PatternFly source and cutting it to 52 words. I posted it on 2026-11-09, a week after the question.

> **lordrip, 2026-11-02:** Hi @jeff-sp, thanks for the thorough analysis. If I understood correctly while checking the code, this PR brings the original `withContextMenu` from Patternfly and adjust it to avoid the issue you described, is that the case?

> **jeff-sp, 2026-11-09:** Yes. I replaced PatternFly's `withContextMenu` on nodes and groups with a custom wrapper that renders the same `ContextMenu` component. The wrapper builds the menu actions synchronously and gives each right-click its own keyed `ContextMenu` instance. The before and after timings for a second right-click are in the trace in the PR description.

### Merged (2026-11-11)

Later, a second maintainer assigned me the issue, confirmed my choice of starting point, approved, and merged the same day. The issue closed with the merge.

> Hi @jeff-sp, thanks for the investigation and for the PR. I'm assigning the task to you. Starting from the cy.click() failed because the page updated failure was the right call.

> Thanks for your contribution! I'm approving the PR and merging

## Limitations and what's next

1. I never saw the CI failure myself, and I realized after raising the pull request that I did not have permission to manually trigger any GitHub workflows/actions. The fix is based on the DOM trace and the unit tests. Only later nightly runs can show the flake is gone.

2. The per-owner cap runs before the fit ordering, so a good-fit issue can lose its slot to a worse one from the same owner (noted in scan-notes.md).

What I would do differently is select an issue that does not depend on CI to show the failure knowing I may not have permission to re-run CI. What's next for this issue is to check the nightly runs for `stepPaste` after the merge.