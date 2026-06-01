# Broker Exam — Project Notes (self-contained memory)

> **Memory rule for this project:** everything is recorded **here, in this folder only**.
> Do NOT write anything about this project to global Claude memory (`~/.claude/.../memory/`)
> and do NOT link it to any other project (Tafuros / web-leads / Revaudio). Standalone.

## What this is
Interactive practice tests for the **Israeli real-estate broker licensing exam**
(בחינת רישוי מתווכים במקרקעין — רשם המתווכים, משרד המשפטים).
**Multi-test now live:** a top-of-page picker switches between sets. Each set keeps the official
format (25 Qs · 90 min · pass = 60 = 15/25) and the HE↔RU toggle.
- **מבחן 1 · בסיסי (Basic)** — the original 25.
- **מבחן 2 · מתקדם (Advanced)** — 25 harder questions (added 2026-06-01): scenario-based, exact-rule
  traps, and less-common topics (תקנת השוק ס'10, עסקאות נוגדות ס'9, חישוב ליניארי מוטב, היטל השבחה,
  הפקעה, דייר ממשיך, מימוש משכנתא). Correct answers spread evenly A–D (6/6/7/6) so position-guessing fails.
More sets / a rotating bank can follow.

## Where it lives
- **Folder:** `~/Desktop/broker-exam/` (its own git repo; intentionally on Desktop, not in `~/projects/`).
- **GitHub:** `danavivimusic-hue/broker-exam` (public) — account stays `danavivimusic-hue`.
- **Live link (GitHub Pages):** https://danavivimusic-hue.github.io/broker-exam/
  This URL is stable — it never changes across updates. This is the link to share.

## Deploy workflow
Edit `index.html` → commit → `git push origin main` → Pages auto-rebuilds the **same URL** in ~1 min.
(Open in Safari/Chrome, not a messaging-app in-app preview, so the JS runs.)

## Build pattern (single self-contained `index.html`, no dependencies, works offline)
- Data model: `TESTS = [{name:{he,ru}, Q:[{a:<correctIndex>, he:{q,o[],e}, ru:{q,o[],e}}]}]`.
  `testIndex` selects the active set; `let Q = TESTS[testIndex].Q` is reassigned on switch. `UI = {he,ru}` holds every interface string.
- **Test picker** (`.tests` / `.testbtn`, built by `renderTests()`): one button per set, localized via `name[lang]`.
  `switchTest(i)` swaps `Q`, resets answers + timer, re-renders, scrolls to top.
- Counts are **derived, not hardcoded**: `passMark() = ceil(Q.length*0.6)`; `UI.count(n)` / `UI.pass(p,n)` / `UI.subFail(pct,p)` are functions. Adding a set of any length Just Works.
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
Append an object to `TESTS`: `{name:{he:'…', ru:'…'}, Q:[ …questions… ]}` (same `{a, he:{q,o,e}, ru:{q,o,e}}`
shape). The picker button, scoring, and pass-mark all derive automatically — no other code changes.
Then commit + push — same live URL. (Or build a bank and draw N at random for "new test".)
