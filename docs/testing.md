# Local test report

**Date:** 3 October 2026  
**Environment:** Windows, Python 3.12, Node.js 24, local FastAPI / Vite / Ollama  
**Source baseline:** `274624f90cec4fe4a0c2cbf28af9b0e544d548c3` in the development repository, plus local corrections described below.

This report covers the local application used for the screenshots. It is not a claim that the unchanged remote source has passed the corrected suite.

## Automated checks

| Check | Command | Result |
|---|---|---|
| Initial backend suite | `python -m pytest tests --tb=short` | 216 passed, 3 failed |
| Targeted regression and contract checks after corrections | Selected schema, API, report and import-job test modules | 40 passed |
| Full backend suite after corrections | `python -m pytest tests --tb=short` | **221 passed** |
| Frontend tests | `npm run test` | **6 passed** |
| Production frontend build | `npm run build` | **Passed** |

## Corrections made locally

### Demo profiles were being treated as stale Takeout imports

The API applied Takeout parser-schema checks even when the active profile came from the anonymous demo, including when no Takeout import existed. That made the interface request a re-import instead of showing valid demo analytics.

The correction applies import-migration status to an active real Takeout import and permits demo analytics to load. Two regression tests cover a demo without an import and a demo alongside an older stored import. The genuine stale-import guard remains enforced.

### Current-month fixtures depended on the actual calendar

Three tests used fixed July 2026 records while asking for the current month. Running them in October made the expected rankings empty. The tests now fix their reference date to the intended July test period. Production date handling was not changed.

These changes remain in the local checkout. No source changes were pushed to either existing Saville repository. This public repository contains presentation material only.

## Browser walkthrough

| Flow | Observation |
|---|---|
| Anonymous demo | Loaded successfully after the API correction |
| Overview period | Changed from 2 current-month plays to 25 rolling-year plays |
| Top 10 | Loaded song and artist rankings with play counts and period labels |
| Insights | Loaded duration summary, listening scores, leaders, coverage, and rhythm |
| Rhythm controls | Weekly and monthly chart content changed with the selected interval |
| Persona Report | Five report chapters rendered with deterministic facts and rankings |
| Regenerate | UI confirmed local Gemma success; API metadata confirmed Gemma generation and subsequent cached retrieval |
| Recommendations | Generated 20 anonymous candidates in lanes of 8 / 8 / 4 |
| Console sample | Three.js clock deprecation warnings; no errors in the captured sample |

## Known limits and untested paths

- Fictional demo artists are not in the canonical genre registry, so genre coverage is 0%. The dashboard visibly reports that limitation.
- Overview fallback copy currently prints `unknown` as a sound description when classification is absent. The wording needs improvement.
- The duration summary estimates detected music minutes from metadata, not actual wall-clock listening time.
- Account authentication, fresh personal history uploads, Spotify live flows, and playlist creation were not exercised in the browser. Automated tests cover related contracts but do not establish live account success.
- Backend tests emitted a TestClient/httpx deprecation warning. The production build emitted large-chunk warnings. Neither prevented completion.
- This was a functional walkthrough, not load, security, usability, or recommendation-quality testing.

## Screenshot provenance

All screenshots are actual local-app captures using built-in fictional tracks and artists. No personal listening export, account credential, database, or raw application source is published in this presentation repository.
