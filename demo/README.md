# Saville demo data

## Importable sample

[takeout-sample.json](takeout-sample.json) contains **30 synthetic listening events across six songs** selected from Chan Zhi Hong's favourites. Song titles, artist names and public YouTube IDs are real; every timestamp and repetition in this file was generated for the demo. It is not a raw personal-history excerpt.

1. Run the development application using its setup instructions.
2. Open Settings and choose **Choose Takeout file**.
3. Select `takeout-sample.json`; then view July 2026 or All History.
4. Expect 30 music plays and six recording IDs. Metadata coverage begins with the import and existing caches; optional catalogue lookups can add durations and release years.

Use a separate local data directory if you already have a profile: set `SMP_DATA_DIR` and `SMP_DB_PATH` to a demo folder before starting the backend. Importing replaces the active local profile.

## Published analysis

[listening-summary.json](listening-summary.json) provides owner-approved aggregate rankings and coverage from the full historical import. It is separate from the small synthetic fixture and does not contain individual listening timestamps.

Screenshots in the README come from the full imported profile. The sample will not reproduce their whole-history totals or exact report text. The archive ends in July 2026; later months are outside its evidence. No raw Takeout archive, account credentials or local database is included.

[persona-report.json](persona-report.json) is the actual rolling-year report from the real imported profile, with prompt version 11 and cached Gemma provenance. Its period ends in October, but its input export ends in July; the intervening months are outside the export. This report is an example output, not an import fixture.
