# Reveille

**A personalized military-calisthenics + conditioning + running PWA.** Build a plan from your own numbers, train with a guided player, target specific muscles, and track progress. No gym, no equipment required. A leaner, sharper alternative to BetterMe, with a belly-fat recomposition focus.

**Status:** working prototype (single-file web app). Next phase: rebuild as a deployed PWA with accounts and sync.

## What it is

Reveille generates an 8-week bodyweight training block tuned to the user's measured baseline (max push-ups, max pull-ups, plank hold, run level), folds running into the week, and drives a lean recomposition with conditioning finishers. Everything runs client-side; the prototype stores all data in the browser via localStorage.

## Current status (prototype)

The prototype is a single self-contained HTML file (reveille.html, to be added to this repo). It is fully functional and runs entirely in the browser. It has been published as a private Claude artifact during development. Live prototype (private): https://claude.ai/artifact/HmwgyZdeSqFH4hFTXGvyEr . To see it populated, open it and choose "Explore the demo" or "Load sample data"; this seeds about 3 weeks of history so Progress and the muscle maps are filled.

## Features built so far

**Onboarding and plan generator.** Intake (level, baseline test, days per week, equipment, runs) generates an 8-week program. Working reps come from the user's maxes (about half of max per set) and climb weekly; weeks 4 and 8 are lighter deloads. The split adapts to days per week, 3 to 6.

**Guided workout player.** Steps through each set: target reps, log actual reps, rest countdowns with audio, timed holds (plank, wall sit), and a conditioning finisher (AMRAP). The run player has a start/stop timer, logs distance and time, and computes pace.

**Running integrated.** Easy, interval and tempo runs are scheduled into the week, scaled by running level and week.

**Progress tracking.** Streak, sessions, total time and PRs; theme-aware SVG charts (weekly volume, push-up best set, run pace, body-weight trend); body-weight logging.

**Exercise library, 30 moves.** Each exercise has a curated form-demo video (real, verified YouTube IDs), a how-to cue, and a common-mistake note. Video embeds and plays inline automatically once the app is hosted on its own domain (auto-detected via window.self === window.top); in the Claude preview it opens the exact video instead, because the sandbox blocks embeds.

**Interactive muscle map (the differentiator).** Tap muscles on a front/back body diagram to select target areas, pick a goal (strength, build, tone or endurance), and it generates a focused workout that hits exactly those muscles, saved under "Your focused workouts" and startable in the player. Passive muscle maps also appear on Today, Progress, and each exercise detail. All drawn from the app's own exercise-to-muscle tags (EXMUS), so there is no third-party data or license dependency.

**CSV import.** Import existing history from Strong, Hevy or FitNotes CSV exports. Imported sets count toward streak and totals.

**Landing and homepage.** Auto-sliding hero carousel (4 designed slides, swipeable), a What's Inside feature showcase, and entry buttons (Build my plan, Explore the demo). The REVEILLE logo is a home button.

**Demo mode.** One-tap sample data so the app can be explored fully populated.

## Architecture notes

Single file, vanilla JS, no framework. CSS custom-property design tokens with full light and dark theming. Persistence is localStorage under the key reveille_v1 (profile, logs, weights, custom workouts, current week). All graphics are SVG and CSS (muscle map, body motifs, charts), with no external images because the Claude sandbox blocks them. Plan generation, muscle-intensity mapping and the targeted-workout generator are pure functions driven by the exercise library (LIB) and muscle tags (EXMUS).

## Known limitations and why

These are limits of the Claude artifact preview, not the app design; they resolve once the app is deployed to its own domain. No real images or inline video in the preview: the sandbox blocks external images and iframes, so the hero uses designed vector art (placeholder) and exercise videos open externally; on deploy, swap in HD images and videos play inline. No install or offline yet: needs a web app manifest and service worker on a real host. Data is per-device: localStorage only, no accounts or cross-device sync until the Supabase rebuild.

## Planned production stack (decided)

GitHub (this repo) for code and deploy trigger. Supabase for database, auth (sign-up and login) and storage. Vercel for hosting, HTTPS, install and offline. Flutterwave Payment Link for optional donations (free app, support if it helps). All free to start. Optional: a domain (about 10 USD per year) and Nano Banana / Gemini, or licensed stock, for HD hero images.

## Roadmap and next steps

First, add reveille.html (the current prototype) to this repo. Second, rebuild as a real PWA on the stack above: Supabase auth plus a schema for users, workouts, logs, body-weight and custom workouts with row-level security; migrate the client state model to Supabase so activity syncs across devices; add a web app manifest and service worker for install and offline. Third, inline video turns on automatically when hosted (already wired); optionally self-host branded clips later. Fourth, drop in the Flutterwave Payment Link for donations (Profile and landing). Fifth, replace placeholder hero art with generated or licensed HD images. Sixth, deploy to Vercel on a custom domain.

## Decisions log

Demos: curated YouTube embeds (free, fastest), inline on deploy; earlier stick-figure animations were scrapped as not production-quality. Muscle map: built in-house from our own tags rather than borrowing openGym's (a native iOS/Android app, nothing portable, and it avoids license entanglement for a possibly-commercial product). Backend: Supabase, which bundles DB, auth and storage in one account; Neon plus Clerk was considered, Supabase chosen for simplicity. Donations: Flutterwave Payment Link, which accepts local and international cards and pays out to a supported-country merchant account.

## How to pick this up

Open the live prototype link above and click through it (use demo data) to see current behavior. Pull reveille.html from this repo; it is the complete, working source of truth for the prototype's logic and UI. Then start the production rebuild from the roadmap: stand up the Supabase project, port the data model, then the auth and sync layer. The prototype's pure functions (plan generator, muscle engine, targeted-workout builder) carry over directly.
