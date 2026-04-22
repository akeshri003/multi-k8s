# Ingest Checklist

Use this workflow when a new source is added.

1. Identify the source file in `raw/`.
2. Determine the source type.
3. If the source is audio or video, prepare the smallest useful derived artifact set in `derived/`.
4. Read the source fully enough to understand its structure and central claims.
5. Capture source metadata: title, source type, author if known, date if known, source path.
6. Create or update the source page in `wiki/sources/`.
7. Extract candidate entities, topics, and open questions.
8. Update relevant topic pages.
9. Update relevant entity pages.
10. Create a question page if the source introduces unresolved tensions worth tracking.
11. Update `wiki/index.md`.
12. Append a structured entry to `wiki/log.md`.

Rule of thumb: a good ingest usually touches more than one file.
