---
layout: post
title: "Methodology"
date: 2026-08-10
series: "With AI For AI"
series_order: 2
---

## Keeping it real
After a few Vibe Code sessions with the DeepSeek API it is very easy to see how an enthusiastic individual could lose track of the source code for the solution they are building. Vibe Coding is an informal habit. For a reproducible outcome some guardrails are needed. The key to this is building a methodology.
- Human Validation: Verify the source code. In fact get to know it. Human in the loop is not enough. Human in control is the goal.
- Hardening: This is accomplished by developing a set of SKILLs for your AI agent. These are needed for requirements to specification and specification to implementation. Its an iterative task to get to a point where these SKILLs do the heavy lifting for the methodology.

### AI Skills development
There are a lot of skills out there. I started by looking at 2 in particular. 
- [Matt Pocock](https://github.com/mattpocock/skills){:target="_blank"}
- [Addy Osmani](https://github.com/addyosmani/agent-skills){:target="_blank"}

The Matt Pocock Grill Me skill was a revelation. The iteration over the original prompt by DeepSeek was impressive. 

The other skills were verbose and it was not apparant what value they were adding. To provide a deeper insight into this it's better to develop them from scratch. This is, after all, a learning exercise.

I will focus on the specification to implementation since the Grill Me skill, even in its simplest form, produced a document of implementation slices. Here is the set from a session on adding OAuth to the expense tracker:

### OAuth + BFF Design (grilling session outcome)
> **Status: implemented.** All slices are done and green:
> 1. **Core per-user isolation** — owner-keyed store/business layer, `ICurrentUser` seam,
>    per-user sessions (implemented + deterministic tests).
> 2. **BFF auth plumbing** — OpenIdConnect + cookie auth, `/login` `/logout` `/api/user`,
>    `[Authorize]`, current-user middleware.
> 3. **Dev identity provider** — **Keycloak in Docker** (option 3, chosen after OpenIddict was
>    ruled out: no OpenIddict version since 3.x has in-memory stores, and all versions hard-require
>    HTTPS — see the blocked-attempt note below). Users/clients live in a realm import JSON;
>    the BFF's `AUTH_AUTHORITY` points at it.
> 4. **Frontend** — login gate, user header, sign-out, 401 handling.
> 5. **Tests** — `ExpenseAgent.WebApi.Tests`: deterministic fake-auth BFF tests + a real
>    Keycloak e2e login test (skips when the container isn't running).
> The full suite is green with and without the Keycloak container.

