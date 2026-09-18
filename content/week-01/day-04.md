+++
title = "Day 04 - 18/09/2026 (REMOTE) Landing Page Refactor"
weight = 4
+++

### Day 04 Report

## A. Work Completed

- Refactored the landing page from a single large Next.js page into a component-based structure.
- Split the page into dedicated components for the header, hero, overview, capabilities, use case, guide, safety, comparison, and footer sections.
- Moved the language state and composition logic into `components/landing/LandingPage.tsx`.
- Removed the inline language switcher styling from the page and moved it into `app/globals.css` without changing the overall visual design.
- Extracted the repeated arrow icon into `components/ui/ArrowIcon.tsx` for reuse.
- Renamed the content source file from `landing-mock-data.tsx` to `landing-data.tsx` while preserving the real bilingual content.
- Added typed structures such as `LandingContent`, `ContentCard`, `ComparisonRow`, and `Language` to improve maintainability and safety.
- Kept the default language as English and preserved the existing anchor navigation, smooth scrolling, and section IDs.
- Verified the project by running `npm run lint` and `npm run build` successfully.

## B. Challenges Encountered

### 1. Breaking a large page into smaller, clearer sections

The original landing page contained all the JSX, bilingual copy, and state logic in one file. This made the page hard to read, maintain, and extend as more content was added.

The solution was to separate the page into distinct section components so each one handles a single responsibility while the landing page acts only as the composition layer.

### 2. Keeping behavior unchanged while improving structure

The page relied on a language switcher, anchor navigation, and a stable layout. Refactoring the structure risked breaking these interactive behaviors or altering the final UI.

To avoid this, the team kept the same content, section IDs, default language, and CSS rules while moving only the composition and state logic into a higher-level component.

### 3. Managing bilingual content cleanly and safely

The landing page had large amounts of English and Vietnamese content, and keeping it inline in the page made updates difficult and error-prone.

The fix was to centralize the data in `data/landing-data.tsx` and define clear TypeScript types for the content structure. This made it easier to add new sections or adjust translations without affecting the layout.

### 4. Preserving responsive behavior during the refactor

Because the page used repeated classes, spacing rules, and responsive layout logic, moving sections into separate files could easily lead to inconsistent styling or broken mobile layouts.

The refactor kept the same class names, breakpoints, and responsive rules, so the final UI remained visually identical to the original design.

## C. Result

The landing page is now more maintainable, scalable, and easier to extend while keeping the same interface, bilingual experience, and production-ready behavior intact.
