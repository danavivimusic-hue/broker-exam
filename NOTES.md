# Broker Exam — Project Notes (self-contained memory)

> **Memory rule for this project:** everything is recorded **here, in this folder only**.
> Do NOT write anything about this project to global Claude memory (`~/.claude/.../memory/`)
> and do NOT link it to any other project (Tafuros / web-leads / Revaudio). Standalone.

## What this is
Interactive practice tests for the **Israeli real-estate broker licensing exam**
(בחינת רישוי מתווכים במקרקעין — רשם המתווכים, משרד המשפטים).
**This is the first of many tests** — more test sets / a rotating question bank to follow.

## Where it lives
- **Folder:** `~/Desktop/broker-exam/` (its own git repo; intentionally on Desktop, not in `~/projects/`).
- **GitHub:** `danavivimusic-hue/broker-exam` (public) — account stays `danavivimusic-hue`.
- **Live link (GitHub Pages):** https://danavivimusic-hue.github.io/broker-exam/
  This URL is stable — it never changes across updates. This is the link to share.

## Deploy workflow
Edit `index.html` → commit → `git push origin main` → Pages auto-rebuilds the **same URL** in ~1 min.
(Open in Safari/Chrome, not a messaging-app in-app preview, so the JS runs.)

## Build pattern (single self-contained `index.html`, no dependencies, works offline)
- Data model: `Q = [{a:<correctIndex>, he:{q,o[],e}, ru:{q,o[],e}}]`; `UI = {he,ru}` holds every interface string.
- **Bilingual 🌐 button, fixed top-right** (`.langbtn`): shows the *other* language; one press flips
  questions, options, explanations, UI, and result, and sets `document.documentElement.dir`
  (he = rtl, ru = ltr). Answer letters localize: HE `א–ד`, RU `А–Г`.
- Features: 90-min countdown (auto-submits at 0, reddens under 5 min), live "answered N/25",
  check button → per-question green/red grading + explanation reveal + conic-gradient % ring,
  reset = fresh attempt. Switching language preserves answers + graded state.
- Use CSS logical properties (`border-inline-start`, `margin-inline-start`) so layout flips LTR/RTL cleanly.

## Israeli broker-exam facts (verified 2026-05-31, official sources)
- 25 American-style questions, one correct each; ~90 min; **pass = 60 = 15/25**; ~60% pass rate.
- 4 sittings/year: Nov, Feb, May, Aug (Jerusalem + Tel Aviv only).
- Syllabus = *תקנות המתווכים במקרקעין (נושאי בחינה), תשנ"ז-1997*.
- Question-mix weighting: **highest** = חוק המתווכים + its תקנות (incl. new אתיקה וחובות מקצועיות תשפ"ד-2024);
  **high** = חוק המקרקעין, חוק החוזים, מיסוי מקרקעין; **medium** = הגנת הצרכן/דייר, מכר דירות, ירושה;
  **long tail** = תכנון ובנייה, עונשין, הוצל"פ, שמאות, רישוי עסקים.
- High-yield anchors (confident): ס'9 הזמנה בכתב + בלעדיות (no-period = 30d, max = 6mo, lapses at ⅓ if no marketing actions);
  ס'10 עניין אישי (disclose + written consent); ס'14(א) three cumulative fee conditions
  (valid license + written order + effective cause / גורם יעיל); חוק המקרקעין ס'7 (deal completes on registration) + ס'8 (writing requirement).

## To add a new test set
Duplicate the `Q` array structure with fresh questions (keep HE+RU + correct index), or build a
question bank and have "new test" randomly draw 25. Then commit + push — same live URL.
