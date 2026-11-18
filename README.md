# Counter: Chromium before / after

Evidence for [AgentCityBuilders/prooflane-fork-benchmark-opencode#1](https://github.com/AgentCityBuilders/prooflane-fork-benchmark-opencode/issues/1).

| Recording | Tested commit | Fresh count | One **Add one** click |
| --- | --- | --- | --- |
| [Before (MP4)](before.mp4) / [original WebM](before.webm) | `838b1a555e59091607ce412b737a19058386d0cd` | 0 | **2** |
| [After (MP4)](after.mp4) / [original WebM](after.webm) | `f2fa68f6f8683f027c8d4076555c63c28931866c` | 0 | **1** |

Both recordings are actual visible Chromium 159.0.8154.6 page recordings at 1280×720, not simulated screenshots. Each navigates to the local server, shows count 0, clicks the same button once, and holds the resulting count on screen. There are no overlays, cuts, or speed changes. The MP4s are H.264 transcodes of the original WebMs for playback compatibility. Playwright records page content, not the browser chrome or OS pointer.

## Reproduce

1. Check out either commit above.
2. Run `node --check server.mjs`, then `node server.mjs`.
3. Open `http://127.0.0.1:3000/` in Chromium.
4. Confirm count **0**; click **Add one** once.
5. Observe **2** on the base, **1** on the candidate.

## Checks

- `node --check server.mjs`: passes on both commits.
- `node --check journey.mjs`: passes on the candidate.
- Recorded one-click behavior assertion: base **0 → 2**, candidate **0 → 1**; both pass.
- Candidate `journey.mjs` against the pinned base: fails as expected (expected 1, received 2).
- Candidate `journey.mjs` against the candidate: passes (initial 0, first click 1, second click 2).
- No page JavaScript errors during either recording.

See [test.log](test.log), [before facts](before.json), [after facts](after.json), and [SHA256SUMS](SHA256SUMS).

## Fresh independent native review

The isolated OpenCode review inspected exact HEAD `f2fa68f6f8683f027c8d4076555c63c28931866c`, independently reran base/candidate browser behavior and the regression, decoded all four videos, and inspected transition frames. Its initial missing-PR finding was resolved after [draft PR #2](https://github.com/AgentCityBuilders/prooflane-fork-benchmark-opencode/pull/2) was created.

The same fresh review session then independently verified the PR's draft/cross-repository destination and source, exact base/head, two-file patch, PR body, anonymous video bytes/hashes, and complete Chromium playback. **No remaining findings.** See [final review report](fresh-review.md), [anonymous access checks](anonymous-access.json), and [anonymous playback checks](anonymous-playback.json). Raw native review events and session identity remain local with the receipt.

## Scope and limitations

This is Chromium-only local verification, not a cross-browser or production deployment test. GitHub CLI has no supported native inline video upload in this environment; these durable public-fork Git links are the fallback, not native PR attachments. GitHub's Markdown UI may offer a download rather than inline playback. Temporary runners, logs, and media are excluded from the implementation branch. The original repository contains no dependency manifest or standalone journey runner; its existing journey contract is exercised with the locally available Playwright Chromium/expect library.
