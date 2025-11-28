This is a [Next.js](https://nextjs.org) project bootstrapped with [`create-next-app`](https://nextjs.org/docs/app/api-reference/cli/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/app/building-your-application/optimizing/fonts) to automatically optimize and load [Geist](https://vercel.com/font), a new font family for Vercel.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/app/building-your-application/deploying) for more details.



# Next App (Hindi News) — Documentation

This small docs set covers how the app is structured, how to run it locally, and common troubleshooting tips.

Quick Start
1. Install:
   npm install
2. Dev:
   npm run dev
   Open http://localhost:3000
3. Build:
   npm run build
   npm start

Structure
- app/
  - components/ — project UI components (FallbackImage, HeroArticle, NewsCard)
  - articles/[id]/page.tsx — article details page
  - page.tsx — home/landing
  - api/news — news API route
- public/ — static assets (add fallback.jpg here)
- docs/ — this folder

See components.md, development.md, and troubleshooting.md for more details.

# Components

This file details important UI components and usage examples.

FallbackImage (app/components/FallbackImage.tsx)
- Purpose: Wraps next/image with fallback behavior on error or invalid image.
- Important: This file is a client component (has "use client";), exported as default.

Usage Example (from an article page):
```tsx
// from app/articles/[id]/page.tsx
import FallbackImage from "../../components/FallbackImage";

<FallbackImage
  src={article.image ?? "/fallback.jpg"}
  alt={article.title}
  width={1200}
  height={600}
  className="w-full rounded-lg shadow-md"
/>
```

Notes:
- Add public/fallback.jpg for fallback asset.
- If you use external images, add the domains to next.config.js:
```js
// next.config.js
module.exports = {
  images: {
    domains: ['upload.wikimedia.org', 'example.com'],
  },
};
```

HeroArticle (app/components/HeroArticle.tsx)
- Renders the hero article on the home page.
- Import usage:
```tsx
import HeroArticle from "./components/HeroArticle";
<HeroArticle article={hero} />
```

NewsCard (app/components/NewsCard.tsx)
- Rendered as a grid item in the home page list.
- Usage:
```tsx
import NewsCard from "./components/NewsCard";
<NewsCard article={article} />
```

# Development & Build

Commands
- Install:
  npm install
- Dev:
  npm run dev
- Build:
  npm run build
- Start (production):
  npm run start

Next Data Fetching
- app/page.tsx uses server components & `fetch` in functions like `getArticles()`:
  - Example: revalidate: 60 for ISR-like behavior in App Router:
    fetch(`${base}/api/news`, { next: { revalidate: 60 } })

Dev server issues
- If TypeScript or VS Code fails to pick up changes:
  - Restart TS server: Ctrl+Shift+P → "TypeScript: Restart TS Server"
  - Stop dev server, remove .next, and restart:
    ```powershell
    Get-Process node -ErrorAction SilentlyContinue | Stop-Process -Force
    Remove-Item -Recurse -Force .next
    npm run dev
    ```

Assert Type Safety
- Run:
  npx tsc --noEmit
  npm run build

Linting & Testing (if set up)
- Add lint/test commands to package.json and run:
  npm run lint
  npm test

  # Troubleshooting

Cannot find module '../../components/FallbackImage'
- Confirm the file exists:
  PowerShell:
  ```powershell
  Get-ChildItem -Path .\app\components -Force
  ```
- Ensure import path is correct (relative to current file):
  - app/articles/[id]/page.tsx → import FallbackImage from "../../components/FallbackImage";
  - app/page.tsx → import HeroArticle from "./components/HeroArticle";
- Check filename case: Windows is case-insensitive, CI or some tooling may be case-sensitive.
- Restart TypeScript server or editor:
  Ctrl+Shift+P → "TypeScript: Restart TS Server"

TypeScript / Declaration errors
- If TypeScript claims no type declarations for a component:
  - Ensure it's .tsx and default exported.
  - As a last resort, add a small declaration:
  ```ts
  // global.d.ts
  declare module "@/app/components/FallbackImage";
  ```

Image issues / 403 from next/image
- Add remote domains to next.config.js images.domains
- For development, FallbackImage uses `unoptimized` already, which can help bypass loader errors.

File delete or slow delete on Windows
- OneDrive may be locking files. Pause sync or move the repo out of OneDrive while developing.
- Check for open handles:
  - Sysinternals: handle.exe "path\to\file"
  - Kill Node if Next dev server is holding it:
    ```powershell
    Get-Process node -ErrorAction SilentlyContinue | Stop-Process -Force
    ```
- If you must delete from terminal:
  ```powershell
  Remove-Item 'C:\Users\sahut\OneDrive\Desktop\next-app\app\articles\[id]\page.tsx' -Force
  ```

Common Fixes
- Restart dev server and TS server
- Delete .next folder and re-run
- Fix relative import path and case
- Add `public/fallback.jpg` and check image path in component