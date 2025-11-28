PART B-
Explanation + Documentation
1. Design Document
✔ Wireframe (conceptual)

Structure followed:

Navbar

Hero Top Headline

Latest Updates Grid

Category Sections (each contains 6 articles)

Footer

This mirrors LiveHindustan’s structure: clear hierarchy → hero → lists → categorized news.

✔ Layout Decisions

Hero section kept large and eye-catching

Latest updates placed below hero

Three-column grid for desktop, single column for mobile

Category sections stacked vertically for easy scroll navigation

Chosen red for highlights (matches Hindi news design identity)

2. Data-Fetching Strategy
Strategy	Why Chosen?
ISR (Revalidate 60 sec)	Best balance of freshness + performance. No need full SSR.
Dynamic Routing	Articles + category pages require dynamic parameters.
API Routes	Keeps API key secure and avoids client-side exposure.

Tradeoffs:

API rate limit may restrict responses

Need fallback content when API fails

3. Code Explanation (Major Components)
✔ /app/page.tsx

Home page

Fetches:

Top news

Category news

Renders:

HeroArticle

Latest Updates Grid

Section-wise news (World, Nation, Business, etc.)

✔ HeroArticle.tsx

Displays hero/top story section

Large image, title, summary

Links to external news

✔ NewsCard.tsx

Reusable news card

Shows title + description + image

Supports external links

✔ CategorySection.tsx

Reusable block displaying a grid of category-wise news

Takes props: title, articles, id

✔ FallbackImage.tsx

Handles missing or broken images

Keeps UI consistent

✔ Header.tsx

Navigation bar

Smooth scroll

Dark/Light mode toggle

Mobile hamburger menu

✔ Footer.tsx

Simple footer with credits

✔ /api/news

Fetches top headlines from GNews

Adds fallback, id mapping

✔ /api/category/[slug]

Fetches category-wise articles

4. Data Model

Each article object follows:

{
  id: number;
  title: string;
  description: string;
  image: string;
  url: string;
  source: string;
  publishedAt: string;
}


Structured to match GNews → normalized for UI usage.

5. Challenges & Solutions
🔴 Challenge 1 — Missing images / broken URLs

Solution: FallbackImage component to display placeholder.

🔴 Challenge 2 — API rate limits

Solution: ISR + error screens + fallback content.

🔴 Challenge 3 — Dark/Light mode flicker

Solution: Using CSS variables + useDarkMode() persistent hook.

🔴 Challenge 4 — Smooth scrolling not working

Solution: Added stable HTML ids + scroll-behavior: smooth; globally.

🔴 Challenge 5 — External images not loading

Solution: Added all image hostnames into next.config.js.

🔴 Challenge 6 — Section backgrounds in dark/light mode

Solution: Use bg-[var(--background)] and text-[var(--foreground)].

6. Improvements If Given More Time

Implement pagination or infinite scroll

A full search page

Authenticate users

Bookmark or save articles locally

Add Hindi language support

Add caching layer in API routes

🅒 — Part C: Testing & Edge Cases
✔ 1. Article Without Image

Expected behavior:

Show fallback /fallback.jpg

✔ 2. API Returns Empty

Home page shows:

No news available. Please try again later.

✔ 3. API Fails (Rate Limit or Timeout)

Graceful error page + fallback content.

✔ 4. Long Titles

Handled using Tailwind:

line-clamp-2;

✔ 5. Responsive Testing

Checked on:

iPhone 14 Pro

Pixel 7

720p tablet

1080p laptop

1440p wide monitor

✔ 6. Broken category slug

Shows:

Invalid category

🅓 — Part D: AI Use & Reflection
✔ Where AI Helped

Component boilerplate generation

Styling suggestions (Tailwind utility classes)

Fixing complex Next.js data fetching issues

Dark/light mode refinements

Documentation writing

✔ Where AI Was Wrong

Suggested incorrect Tailwind classes (e.g., bg-(--background) instead of bg-[var(--background)])

Provided outdated Next.js routing approaches

Some image optimization suggestions incompatible with external domains

✔ How I Verified & Corrected AI Output

Checked Tailwind documentation for proper variants

Tested all UI components on mobile + desktop

Used console logs to verify API responses

Manually fixed dark mode flicker

Replaced wrong code with stable custom hooks

✔ Custom Work Beyond AI

Created complete scroll-navigation system

Built full category system from scratch

Proper article model normalization

UI improvements matching news style

Error boundary + fallback logic

Smooth polished theme system

🎉 Conclusion

This project demonstrates practical skills with:

Next.js App Router

API Integration

Responsive UI Design

Component Architecture

Dark/Light Mode

SEO Optimization

Error Handling

It meets all company requirements, is production-ready, and reflects a clean engineering approach
