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
