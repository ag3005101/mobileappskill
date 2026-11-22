---
name: mobileappskill
description: Use ONLY when the user explicitly says they want to build, start or design a mobile app (e.g. "I want to build an app"). Never for routine chats or general questions.
---

# Mobile App Blueprint (Android + iOS, non-coder + Claude)

Created by: **CGST vala Aditya**

User is a non-coder in India building with Claude. Explain in plain words, decide technical details for them, and confirm only product decisions.

**How to use**
- New project: run `docs-first-project-setup` first, if that skill is installed. Feed this checklist into its PRD, DESIGN_SYSTEM, SECURITY and TESTING docs.
- Load only the section the current stage needs. Don't recite the whole file.
- Facts dated Nov 2026. Before quoting fees, free-tier limits, build floors or legal dates, re-check them with one quick web search.

## Default stack (change only for a clear reason)
- **App:** React Native + Expo (TypeScript). AI writes it reliably, EAS builds iOS without a Mac, and OTA updates are built in. Flutter only if pixel-identical custom visuals are the priority.
- **Backend:** Supabase (SQL, auth, storage, Edge Functions) or Firebase (realtime/offline). Never a custom server.
- **Auth:** platform auth with email/OTP + Google. On iOS, any third-party login also needs Sign in with Apple (4.8).
- **Version control:** Git + GitHub from day 1, commit after every working change. Use the `gh` CLI, not the GitHub MCP (token-heavy).

## Stage checklist
1. **Idea:** one-line problem + target user. Check 5–10 rivals' 1–3★ reviews. Talk to 10+ users or run a waitlist. Choose the money model early (free/ads/subscription/one-time).
2. **PRD:** MVP = 3–5 features max, plus out-of-scope items and success metrics. Decide whether accounts are needed (if yes: in-app deletion). List all personal data collected and all permissions (ask each permission in context, with a specific reason).
3. **Design:** user flows (include signup, login, logout, delete account, settings, help) → grey wireframes → mini design system in DESIGN.md (1 primary colour + neutrals, type scale, 8-pt spacing, buttons/inputs/cards) → clickable prototype tested with 5 users.
   - Every screen has 4 states: loaded, empty (with a call to action), loading (skeleton), error (plain message + Retry).
   - Onboarding: max 3 skippable screens; sign-up only after the user has seen value.
4. **Build:** one feature at a time: prompt → run on phone (Expo Go/dev build) → test → commit. Test on a cheap Android, a large phone and an iPhone, with slow network, denied permissions and the largest font. Add crash reporting + 3–5 analytics events before launch.
5. **Test tracks:** iOS TestFlight. Android internal test → closed test with **12 testers × 14 continuous days** (new personal accounts). Recruit testers while building and allow ~3 weeks.
6. **Launch + maintain:** real screenshots, support URL, privacy policy URL, demo login in review notes. Weekly crash check. Yearly SDK/API-level update is mandatory.

## Design rules (HIG + Material 3)
- Touch targets 48×48 (iOS minimum 44 pt, Android 48 dp, 8 dp gaps). Icon-only buttons get padding + an accessibility label.
- Contrast WCAG AA: 4.5:1 text, 3:1 large text, checked in light AND dark mode (design both from day 1).
- Respect safe areas (notch, Dynamic Island, home bar) and Android edge-to-edge insets.
- Navigation: bottom bar with 3–5 tabs. Never override system back/swipe.
- System fonts. Support Dynamic Type / `sp` scaling. Flexible layouts only (small phones → tablets/foldables).
- One primary action per screen. Button labels say what happens. Colour is never the only signal. Respect Reduce Motion.

## Store + legal gates (missing any = rejection)
- **Accounts:** Apple $99/yr (₹8,700; India enrolls via the Apple Developer app). Google Play $25 once.
- **Privacy policy:** required on both stores (in the listing and inside the app). Apple privacy labels + Play Data safety form must match it and cover SDKs.
- **Account deletion:** started in-app (Apple 5.1.1(v); deactivation doesn't count). Play also needs a web deletion link.
- **Build floors:** Android target API 36. iOS built with Xcode 26 / iOS 26 SDK. Both rise yearly.
- **Payments:** digital goods → Apple IAP / Play Billing (India: user-choice billing with UPI allowed on Play). Physical goods/services → Razorpay/Cashfree/UPI directly.
- **Top rejections:** crashes/placeholder/dead buttons (2.1), no demo login, backend asleep during review, vague permission text, screenshots not matching the app, thin web wrappers (4.2), template copycats (4.3).
- **India DPDP Act 2023 + Rules 2025:** core duties from **13 May 2027**: standalone consent notice per purpose, easy withdrawal, security safeguards, breach notification, deletion when the purpose ends, grievance contact. Under 18 needs verifiable parental consent, so avoid minors at first. Build to DPDP from day 1. Not legal advice: get a lawyer for payments, health or kids' data.

## Security (never skip)
- Everything in the app bundle is public (`EXPO_PUBLIC_*` is plain text). No AI, payment or Supabase service-role keys in the app.
- Secrets only in Edge/Cloud Functions or EAS secrets. `.env` goes in `.gitignore`.
- Supabase: RLS ON for every table, with a tested policy for each. Free tier has no backups and pauses after 7 idle days, so export data or upgrade before real users.
- Tokens go in `expo-secure-store`. Collect the minimum data.

## Claude tools (lean)
- **Claude Code:** writes the app. Keep CLAUDE.md/AGENTS.md short and use a fresh session per feature pointing to PRD.md + DESIGN.md.
- **Expo plugin (official):** `claude plugin install expo@claude-plugins-official`, then `/mcp` to sign in. Must-have.
- **Supabase MCP:** scoped to one dev project, `read_only=true` on real data, approvals on.
- **Design:** use the Design artifact type / Claude Design for key screens only (heavy on usage). Figma plugin only if the user designs in Figma.
- **Tokens:** keep tool search on, remove idle MCP servers, use Sonnet for routine edits and the strongest model for planning/debugging.

## Mistakes to block
Building too much · no PRD/design system/Git · secrets in app or RLS off · happy-path-only design · missing deletion/privacy/Data safety/demo login · forgetting the 14-day Play test · treating launch as the end · ignoring DPDP.
