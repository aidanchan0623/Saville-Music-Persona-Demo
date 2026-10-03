# Technical walkthrough

## 1. The data contract comes first

The application accepts Google Takeout history formats and supports optional YouTube Music and Spotify integrations. Parsing and normalisation turn different input formats into a canonical listening-event representation. Source identity, timestamps, metadata, and provenance let downstream calculations be traced back to a record.

Overlapping copies of an occurrence can be removed without deleting genuine repeated plays. Recording identity keeps versions such as live, remix, acoustic, and remaster separate. Artist and genre enrichment adds evidence where available, rather than assigning every unfamiliar name to a genre.

These are implementation capabilities verified from the source and automated tests. The browser walkthrough used the built-in synthetic dataset, not a newly imported personal export.

## 2. One period, consistent evidence

Period selection uses a configured local timezone. A calendar month, last 30 days, rolling year, and all history have distinct boundaries. The same period profile feeds counts, rankings, listening scores, and report evidence.

In the tested demo on 3 October 2026:

- This Month showed 2 detected plays.
- Rolling Year showed 25 detected plays across 25 active days.
- The leading song, Midnight Archive, had 4 plays.
- The leading artist, Nocturne Vale, had 7 plays across 3 songs.
- The rolling-year duration summary was 1 hour 32 minutes, using usable track-duration metadata.

These values describe a small synthetic fixture. They do not measure product performance or recommendation quality.

## 3. Coverage is part of the answer

Track-duration totals represent detected music minutes, not exact elapsed listening time. Missing durations change coverage and must not silently become zero-length listening sessions.

The synthetic artists have no canonical genre mappings in this run. Insights correctly shows 0% classified and 100% unclassified genre coverage. Genre-dependent scores and interpretations should be read with that limitation in mind.

The Overview fallback still includes the literal word `unknown` in its sound description. Improving that empty-metadata copy is a remaining UI task; the screenshots do not conceal it.

## 4. Local language, fixed facts

The backend first builds deterministic personality, ranking, duration, and release-year evidence. Ollama runs `gemma3:4b` locally to produce a small set of report-language fields. Schema-constrained generation, validation, time limits, and deterministic fallback copy keep the report usable when the model cannot produce an acceptable response.

The tested regeneration succeeded. Its generation metadata reported `gemma3:4b`, with approximately 51 seconds spent generating language on this machine. This is one observed run, not a latency benchmark. A subsequent report read returned `cache-gemma`.

Report cache identity includes the selected source, period, analytics fingerprint, and version information, so prose can follow the evidence it was generated from.

## 5. Recommendations with reasons

The anonymous demonstration produces 20 fictional candidates, split into 8 safe matches, 8 adjacent discoveries, and 4 exploratory picks. Candidate cards display fit and source reasons, including cautious language when genre evidence is limited.

Fit percentages are internal scores, not measured prediction accuracy. Live search quality and account playlist creation were not tested in this walkthrough.

## 6. Implementation stack

- **Frontend:** React 19, TypeScript, Vite, Tailwind, Recharts.
- **Visual presentation:** Motion, GSAP, Three.js and OGL components.
- **Backend:** Python, FastAPI and Pydantic response models.
- **Persistence:** SQLite-backed JSON repository and metadata caches.
- **Integrations:** optional `ytmusicapi`, Spotify OAuth / Web API, and export importers.
- **Language generation:** local Ollama with Gemma 3 4B.

Application setup and raw source are maintained separately from this screenshot presentation.

## Screenshot gallery

- [Overview](../assets/01-overview.jpg)
- [Top songs](../assets/02-top-songs.jpg)
- [Coverage summary](../assets/03-insights.jpg)
- [Gemma Persona Report](../assets/04-persona-report.jpg)
- [Musical Age](../assets/05-musical-age.jpg)
- [Recommendations](../assets/06-recommendations.jpg)
- [Weekly listening rhythm](../assets/07-listening-rhythm.jpg)
