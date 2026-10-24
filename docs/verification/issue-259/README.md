# Tournament overview reading layout (#259)

Verified locally on 2026-10-24 (America/Los_Angeles), against main `f4c7caa`.

Current tournament overviews now reuse the historical `rules-article` surface. The old uniform 16px/48px document typesetting is scoped to Wiki pages, which retain their existing presentation. Tournament body text remains 16px with a 29.6px line height; section headings recover their hierarchy, and rule groups have visible spacing and separators. The desktop index uses the historical handbook's column width and gap, on the right of the document. Its links have 44px minimum height; mobile retains its top index and gains a visible back-to-top link and room below the wrapped header when following anchors.

Rule groups retain their IDs, headings and literal text inside neutral containers, so the existing pastel styling does not turn nested sections into nested cards. No interface messages, competition data, publication checks or preference behavior changed.

## Screenshots

These are local browser captures at 1440×1000 and 390×844, using default appearance, dark theme and Simplified Chinese. The current-edition fixture combines the existing synthetic test edition with the development preview's sample rules; it is not an approved tournament configuration. Historical captures use the committed S10 archive and rulebook as a visual reference.

| View | Before | After | Historical reference |
| --- | --- | --- | --- |
| Desktop | [Current overview](before-desktop-s11.png) | [Current overview](after-desktop-s11.png) | [S10 overview](after-desktop-s10.png) |
| Mobile | [Current overview](before-mobile-s11.png) | [Current overview](after-mobile-s11.png) | [S10 overview](after-mobile-s10.png) |

At 1440px, both current and historical document panels measure 1110px; at 390px both measure 358px. Their different contents and index placement remain intact.

## Verification

- `npm run check`: passed (ESLint, generated route types, TypeScript).
- `npm run build -- --webpack`: passed. The default Turbopack build cannot resolve this Windows worktree's junction to the shared dependency directory; the production browser checks use the successful Webpack build.
- Affected overview, stage and i18n unit checks: 13 passed.
- Existing `appearance-language.spec.ts`, “keeps locale, theme and publication boundaries across public and private navigation”: desktop and mobile passed (2 tests).
- Browser matrix: 54 combinations, covering 320/390/768/1024/1440px, default/cute-pastel/swiss-minimal and light/dark. All three locales were checked at 390px and 1440px; the intermediate widths used zh-Hans. No horizontal overflow, document/index overlap or page errors. All index targets exist, rule links and back-to-top navigation work. See [measured results](matrix.json).
- Twelve scoped Axe WCAG A/AA scans across mobile/desktop and all appearances/themes: zero violations. Keyboard Enter also follows the rules and back-to-top links.
- Wiki smoke check retains its original 16px body/headings and 48px document line height.
- `npm audit --omit=dev`: zero vulnerabilities.

The full unit/integration run was **not green**: 803 passed, 61 failed across 11 files. Failures include unavailable Bash/xz tooling, Windows symlink restrictions and existing media/route test assumptions. The four unexpected Developer/media assertions were rerun on unchanged main and reproduced identically (3 files, 4 failed/5 passed). These failures are not presented as successful verification; Linux CI remains required.

PostgreSQL migrations and fixtures used only the explicitly isolated `oclr_issue259_test` and `oclr_issue259_browser_test` databases on loopback port 55459. No demo or production database/process was used. No deployment, merge or organizer visual acceptance is claimed. Keep the issue open pending review and authoritative CI.
