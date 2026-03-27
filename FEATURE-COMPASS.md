# Feature Compass

> Living registry of product features and their intended outcomes.
> Organized by user journey. Each feature has a canonical name, one-liner
> outcome, acceptance criteria, and status.
>
> This file is the team's shared vocabulary — referenced during planning,
> implementation, and communication. Git-tracked, human-readable, always current.
>
> Managed by the `feature-compass` skill: `/feature-compass`
>
> Built by [Mariana Borssatto](https://github.com/mborssatto) · [Contribute](https://github.com/mborssatto/feature-compass)

**Status flow:** `clarifying` → `building` → `shipped`

---

<!-- DELETE everything below this line and start adding your own features. -->
<!-- The sections below are examples showing the expected format.          -->

## Onboarding Flow

### Welcome Screen Personalization
- **One-liner:** New users see content tailored to their stated goals within 3 seconds of signup.
- **Status:** shipped
- **Acceptance criteria:**
  - [x] User selects goals during signup
  - [x] Welcome screen renders personalized content based on selections
  - [x] Fallback to generic content if no goals selected
  - [x] Page loads in under 3 seconds
- **Out of scope:** ML-based recommendations, A/B testing

### Email Verification
- **One-liner:** Users confirm their email before accessing paid features, reducing fake signups by 80%.
- **Status:** building
- **Acceptance criteria:**
  - [x] Verification email sent on signup
  - [ ] Clicking link confirms the account
  - [ ] Unverified users see a reminder banner
  - [ ] Expired links show a resend option
- **Out of scope:** SMS verification, OAuth-only flows

## Payment Experience

### Smart Retry Logic
- **One-liner:** Failed payments automatically retry with exponential backoff, so users don't lose access due to transient card failures.
- **Status:** clarifying
- **Acceptance criteria:**
  - [ ] Retry up to 3 times on transient failures (5xx, timeout)
  - [ ] No retry on permanent failures (4xx)
  - [ ] User sees "retrying" state, not an error
  - [ ] Admin dashboard shows retry history
- **Out of scope:** Alternative payment methods, offline mode
