+++
title = "Day 03 - 17/09/2026 (REMOTE) Landing Page GPT-6 Astra"
weight = 3
+++

### Day 03 Report

## A. Work Completed

- Built a landing page introducing GPT-6 Astra using Next.js.
- Designed a modern interface with a green-themed visual system, bright background, and responsive layout.
- Completed the main content sections:
  - What GPT-6 Astra is.
  - The capabilities and strengths of GPT-6 Astra.
  - A 4-step registration and usage flow.
  - Safety considerations when using it.
  - A comparison table between GPT-6 Astra and GPT-5.6 Sol.
- Added anchor-based navigation to jump quickly between sections.
- Added bilingual support:
  - English is the default language.
  - Users can switch between EN and VI directly from the navigation bar.
- Moved the bilingual mock content out of the page component and placed it in:
  - `data/landing-mock-data.tsx`
- Updated the page metadata in Vietnamese, including the title and description for GPT-6 Astra.
- Verified the production build successfully with `npm run build`.

## B. Challenges Encountered

### 1. Organizing a long landing page

The landing page needed to include many sections such as introduction, features, instructions, safety notes, and comparison. The challenge was keeping the content flow clear without making the page feel like a long wall of text.

The solution was to divide the content into clearly separated sections, use numbered headings, alternate background colors, and add a comparison table to make the information easier to scan.

### 2. Supporting bilingual content while keeping the layout stable

The English and Vietnamese versions of the content had different lengths. This easily caused misalignment in cards, headings, and tables, especially on smaller screens.

The solution was to use a flexible CSS Grid layout, responsive typography with `clamp()`, and mobile breakpoints to maintain visual balance.

### 3. Separating mock data from the UI

At first, the bilingual content was embedded directly inside `page.tsx`, making the component long and harder to maintain. When adding or updating content or translations, it was time-consuming to find the right location.

The solution was to move the displayed data into `data/landing-mock-data.tsx`. The page component now focuses on rendering the UI and handling language switching.

### 4. Formatting headings within mock data

Some headings needed line breaks or emphasis with italics. When the content was moved into mock data, these text elements had to be represented properly so the design still preserved the intended formatting.

That is why the mock data file was placed in `.tsx` instead of `.ts`, allowing React elements to be included when necessary.

## C. Result

The landing page is now complete with rich content, EN/VI support, responsive behavior across screen sizes, and a clearer separation between UI and data for easier future extension.


[gpt6astra-pi.vercel.app](https://gpt6astra-pi.vercel.app/)
