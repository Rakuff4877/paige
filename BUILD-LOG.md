# Paige Lite: Project 2 Build Log

## Session 1: Monday, November 16, 2026 (in class)

**Idea statement.** Paige Lite is a phone-friendly reading companion based on my larger Paige book platform. I'll use it when I'm deciding what to read next and to keep myself reading every day. Main features: a 3-question quiz that matches me to books, a reading shelf, and a daily reading-streak tracker.

**What I asked Claude Code to do**
- Turn my existing Paige app (which uses the Claude API, Supabase login and a Postgres database) into a version that meets Project 2 rules: one `index.html`, fixed rules only, no AI service, no secret keys.
- Keep Paige's retro poster look: thick black outlines, hard shadows, sun-yellow and coral colors, Anton and Space Grotesk fonts.
- Create the public GitHub repository `paige` (no code pushed yet).

**What it produced**
- `index.html`, a single page with three tabs:
  - **Match:** pick genres, a mood and a pace. Every book in a built-in library of 41 books is scored with fixed points (genre +3, mood +2, pace +1). The top 6 are shown, each with a plain-English reason ("Because it's a mystery pick, it reads as funny and it moves at a fast pace.").
  - **Shelf:** save books as Want to read, Reading or Finished.
  - **Streak:** log pages read today. Shows the current streak, best streak, pages over the last 14 days, and a 14-day calendar.
- Data is saved only in the browser (localStorage). Nothing is sent anywhere.

**What I corrected or decided**
- Chose the "Lite" scope instead of a book-club tracker so the first version stays small and testable.
- Replaced the AI-generated book picks in the original Paige with a transparent points system, so every recommendation can be explained.

**Testing**
- Ran a scripted test: picking Mystery + Funny + Fast returned 6 matches with "Big Little Lies" first. Saving to the shelf and logging 25 pages (streak = 1) both worked.
- Checked the layout at phone width: no sideways scrolling.

**Next step**
- Wednesday: publish this folder to https://github.com/Rakuff4877/paige and turn on GitHub Pages. Then test on my phone at https://rakuff4877.github.io/paige/.

## Session 2: Monday, November 23, 2026

**What I asked Claude Code to do**
- Add a bar chart to the Streak tab showing pages read per day for the last 14 days, built from the pages I already log.
- Label each bar with the day and the page count, show the 14-day total above the chart, and show days with no reading as an empty bar at zero (not a gap).
- Draw it with plain HTML/CSS: no chart library and nothing loaded from the internet, so it still works offline.
- Keep all the Project 2 rules: single `index.html`, no AI, no logins or keys, data only in my phone's browser, retro poster look, no sideways scrolling. Do not reset or rename saved data, and update the offline cache version so my phone gets the new version.
- Show me a plan first and wait for my approval.

**What it produced**
- On the Streak tab, the 14-day calendar grid is replaced by a **Pages per day** chart: 14 upright bars (oldest on the left, today on the right). Each bar has its page count on top and the weekday letter and date underneath. The 14-day total is shown in large type above the chart.
- Days with no reading show a short outlined stub labeled "0" on the baseline.
- Bars are coral with thick black outlines; today's bar is sun-yellow.
- The third stat tile now shows **Pages today** instead of "Pages · 14 days", so the 14-day total isn't shown twice.
- The offline cache version in `sw.js` went from `paige-lite-v2` to `paige-lite-v3`.
- The saved-data key (`paige-lite-v1`) and its format are unchanged, so my existing shelf, streaks and page logs still load.

**What I corrected or decided**
- Chose to replace the calendar grid rather than show the same 14 days twice.
- Chose upright bars over sideways rows. Because bars are only about 20px wide on a phone, counts of 100+ pages use a smaller font so they still fit.
- Chose to highlight today's bar in yellow.
- Chose to test on a local copy on my Mac with sample data, because browser storage is kept separately for each website address and each device, so test data can't touch the real logs on my phone.

**Testing**
- Ran a local copy with 14 days of sample data, including zero days, a 1-page day and a 1,500-page day. All 14 bars appeared; zero days showed as stubs; "1500" fit above its bar; today's bar was yellow. The total (1,719), Pages today (18), current streak (3) and best streak (4) were all correct.
- Checked at 320px and 375px phone widths: no sideways scrolling.

**Next step**
- Open the updated app on my phone, confirm my real logs show as bars and my shelf is unchanged, and check it still opens in airplane mode.

## Session 3: Wednesday, November 25, 2026

**What I asked Claude Code to do**
- Before writing any code, explain exactly how Match scores books (every rule and point value, how ties are broken, why it shows the top 6) and walk through Mystery + Funny + Fast. Then ask me about every choice instead of deciding on its own.
- Make Match explain itself: show each book's point breakdown and add a short "How matching works" note.
- Let me rate any book on my shelf from 1 to 10 with large, thumb-friendly buttons; show "Not rated" when there is none, and let me change or clear it.
- Keep all Project 2 rules, keep my saved data, and update the offline cache version.

**What it explained about the old scoring**
- Genre match +3, mood match +2, pace match +1 (max 6). Books with 0 points were hidden. Ties were sorted by full title (so "The Hound…" sorted under T). It showed exactly 6 results because of a fixed setting chosen for a short phone list.
- For Mystery + Funny + Fast, six books tied at 3 points for 6th place, and only "A Man Called Ove" was shown, purely because "A" comes first alphabetically.
- With 3/2/1, a book matching only my genre tied with a book matching my mood and pace (both 3 points).

**What I decided**
- New weights: genre +4, mood +2, pace +1 (max 7). Every combination now has its own score, so genre always outranks mood, and mood always outranks pace.
- Show the top 6 plus any books tied with 6th place, up to 10, with a note when tied books are left out ("+1 more book also scored 3 points").
- Break ties by title A–Z, ignoring a leading "The", "A" or "An".
- The point breakdown replaces the old "Because…" sentence; the score badge stays. Misses show the book's own value with +0, and skipped questions say "not asked".
- Ratings: a Rate button opens a 5×2 grid of numbers with Clear and Done. Removing a book from the shelf keeps its rating. Rated books also show "Your rating" on Match results.

**What it produced**
- Example line: "Genre: Mystery +4 · Mood: Funny +2 · Pace: Fast +1 = 7 points".
- The "How matching works" box is built from the same point values the code uses, so the note can't drift from the actual rules.
- Ratings are saved inside my existing saved data (same storage key); older saves load normally and just start with no ratings.
- Offline cache version went from `paige-lite-v3` to `paige-lite-v4`.

**Testing**
- On a local copy with sample data (separate from my phone): Mystery + Funny + Fast showed 10 books: Big Little Lies (7), The Thursday Murder Club (6), three Mystery books at 5, then five books at 3, with "+1 more book also scored 3 points" (Project Hail Mary).
- History + Dark + Fast put the four History-only books (4 points) above the dark, fast books from other genres (3 points).
- Skipping Pace showed "Pace: not asked" and a "/6" badge.
- Rated The Martian 8, changed to 3, cleared it ("Not rated"), rated 9, removed it from the shelf and re-added it, and the 9/10 came back. It also showed "Your rating: 9/10" on Match.
- Loaded an older save with no ratings: shelf, streak and the pages bar chart all still worked.
- Rating buttons measured 52px tall; no sideways scrolling at 320px or 375px.

**Next step**
- Test both features on my phone.
