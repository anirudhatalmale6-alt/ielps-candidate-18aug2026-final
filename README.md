# IELPS — post-correction candidate, 18 August 2026

Prepared against *Developer Instruction — 6 August CEFR Design Revocation and
Final Conformance Correction*, 18 August 2026.

Production is unchanged. Nothing in this pack has been deployed.

---

## 1. Release identity

| | |
|---|---|
| Commit | `665fcb389986a7dabbbbdd9d5b6c0ca0d97d7229` |
| Previous candidate | `2b7966ca446919dddd928c5b5e1cf5e292e72624` (superseded) |
| Branch | `visual-direction-20260815` |
| Repository | `10495109/v0-ielps-platform-h4` |
| Build ID | `drlawguMnkYdYOBJgkefJ` |
| Source files inventoried | 125 |
| Manifest hash (SHA-256 of `source-inventory-sha256.txt`) | `551ef51115533decc70c14e0af41bd3258daad0ffb35251fcbd20ae837b1fbde` |
| Deployment status | NOT DEPLOYED — awaiting DEPLOY APPROVED against this identity |

125 files, not 126: two files were removed (`components/access/access-header.tsx`,
`public/images/learners.jpg`) and one added (`lib/developer-surface.ts`).

---

## 2. What changed, and nothing else

Fifteen paths. Every one of them is named by the instruction.

| File | Why |
|---|---|
| `app/access/page.tsx` | §3 — rebuilt through the learner system; the revoked shell and its typeface are gone |
| `app/levels/[level]/page.tsx` | §3 — same, for all six level pages |
| `components/access/cefr-panel.tsx` | §3, §5 — rebuilt in the canonical idiom; watermarked image removed |
| `components/access/level-row.tsx` | §3 — the level card is now the learner system's card |
| `components/access/start-level-button.tsx` | §3 — the primary action is now the system's primary action |
| `components/access/access-header.tsx` | §3 — **deleted**; it existed only to carry the revoked dark header |
| `public/images/learners.jpg` | §5 — **deleted**; watermarked, no licensed source |
| `components/site-header.tsx` | §3 — takes an `away` flag so the rebuilt pages can use the real header |
| `components/app/source-badge.tsx` | §6 — `EndpointChip` is gated at the one place every caller goes through |
| `components/app/panel.tsx` | §6 — panel endpoint label gated with its wrapper |
| `components/app/screen-view.tsx` | §6 — the "Server wiring" drawer is a diagnostic, not learner content |
| `components/pathways-section.tsx` | §6 — the card endpoint strip and the section endpoint chip |
| `components/adult-flow-section.tsx` | §6 — the four step routes, one of which was `/api/progress/lesson` |
| `components/lesson-player/lesson-player.tsx` | §6 — the endpoint strip under each lesson step |
| `lib/developer-surface.ts` | §6 — **new**; the single switch the above read |

Not touched: Public Landing, routing, `/?pathway=` handoff, billing, Stripe,
webhooks, database, vocabulary, PiP, Parent dashboard content, the Keyword,
Reading and Writing work, the EILPS wording and footer wordmark, and the
global, curriculum and PiP typography.

---

## 3. §3 — the ladder and level pages, rebuilt through `/learner/`

The revoked implementation loaded `Plus_Jakarta_Sans` in `app/access/page.tsx`
and `app/levels/[level]/page.tsx` and painted `bg-indigo` behind its own header.
None of that survives — `Plus_Jakarta_Sans` does not appear anywhere in the
source tree any more, and neither page loads a typeface at all. They inherit
what `app/layout.tsx` already supplies to every other page.

They were not repainted and they are not approximations. They use the
components the learner system already runs:

- `SiteHeader` and `SiteFooter` — the panel's own chrome, not a copy of it
- the section pattern from `WelcomeSection`: `border-b border-border`, a
  `bg-background` band alternating with `bg-card/40`, `py-16 lg:py-20`
- the page width `mx-auto max-w-7xl px-5 lg:px-8`
- the card `rounded-xl border border-border bg-card p-5 hover:border-primary/40`
- the pill eyebrow `rounded-full border border-border bg-card`
- the primary action already used by the level band on the panel home

### §3.1 typography

No global token changed, no curriculum change, no PiP change, no other Access
Panel change, no signed-in-app pass. The rebuilt pages simply stopped loading a
typeface of their own, so the layout's Bricolage Grotesque and Inter apply —
measured below.

---

## 4. §4 — the canonical CEFR information hierarchy

Rendered on the ladder and on each level page, in this order:

    A1  →  BREAKTHROUGH  →  Beginner  →  (definition)

The course title is not printed beside the level on either page. It appears as
a course title in the level band on the panel home, which reads:

> **A1 Beginner** — Breakthrough. Course: Foundation English — 8 units and 40
> lessons. The first lesson is "Language Focus: First Words and Classroom
> English".

All four elements are present and distinct there. One note for your decision:
in that band the descriptor sits in the supporting line **after** the name,
rather than between the code and the name as §4's preferred presentation shows.
Every element is correct and separate; only the order within the band differs.
I have left it alone because §8 lists Access Panel Home as preserve-not-redesign.
Say the word and I will reorder it.

Verified live for all six levels — see check 2 in §5 below. The six names,
descriptors and definitions were already correct and were not rewritten.

---

## 5. Functional checks — 12 of 12 PASS

Run against the built candidate with the curriculum answering from the live
server.

| # | Check | Measured |
|---|---|---|
| 1 | No level band without `?level=` | `#your-level` absent |
| 2 | `?level=A1…C2` renders the right band | A1 Beginner · A2 Elementary · B1 Intermediate · B2 Upper Intermediate · C1 Advanced · C2 Proficiency |
| 3 | Exactly one primary "Continue with {LEVEL}" | 1 control |
| 4 | Pathway identity precedes placement | `href=#pathways` |
| 5 | No level-page bypass into the Adult Lesson Player | none |
| 6 | Canonical `/api/…` only, no `/api/eilps` | `/api/auth/me`, `/api/auth/pathways`, `/api/auth/refresh`, `/api/curriculum/deep-catalog` |
| 7 | Unknown level route 404s | HTTP 404 for `/levels/d9/` |
| 8 | No technical endpoint copy on learner surfaces | none, across 8 pages |
| 9 | Ladder and level pages paint the live learner shell | `rgb(249,249,252)` / Inter / Bricolage Grotesque / `rgb(13,0,77)` on all three sampled |
| 10 | Revoked typeface and watermarked image not requested | none |
| 11 | The ladder opens all six levels | a1–c2 all HTTP 200 |
| 12 | Header links on the rebuilt pages resolve off-page | `/`, `/#pathways`, `/#adult-flow`, `/dashboard` |

Build, TypeScript and lint all exit 0.

---

## 6. §14 — measured inheritance, not asserted

Both sides read the same way: Playwright at 1280 wide, computed styles off the
rendered page. Live baselines on the left of each comparison are
`https://eilps.com/learner/…` as it runs today.

| Property | Live `/learner/` | Candidate `/access/` | Candidate `/levels/a1/` | Live parents dashboard | Candidate parents dashboard |
|---|---|---|---|---|---|
| Shell background | `rgb(249, 249, 252)` | `rgb(249, 249, 252)` | `rgb(249, 249, 252)` | `rgb(249, 249, 252)` | `rgb(249, 249, 252)` |
| Body typeface | Inter | Inter | Inter | Inter | Inter |
| Heading typeface | Bricolage Grotesque | Bricolage Grotesque | Bricolage Grotesque | Bricolage Grotesque | Bricolage Grotesque |
| Text colour | `rgb(13, 0, 77)` | `rgb(13, 0, 77)` | `rgb(13, 0, 77)` | `rgb(13, 0, 77)` | `rgb(13, 0, 77)` |
| Header height / position | 73px sticky | 73px sticky | 73px sticky | 61px sticky | 61px sticky |
| Header background | `oklab(0.982955 … / 0.95)` | same | same | `oklab(… / 0.85)` | same |
| Widest inner container | 1216px | 1216px | 1216px | 1024px | 1024px |
| Card border | 1px `rgb(205, 205, 219)` | 1px `rgb(205, 205, 219)` | 1px `rgb(205, 205, 219)` | 1px `rgb(205, 205, 219)` | 1px `rgb(205, 205, 219)` |

For comparison, measured on the **revoked** implementation earlier today: the
ladder and level pages painted a `rgb(13, 0, 77)` dark indigo shell and set
Plus Jakarta Sans over it, against the `rgb(249, 249, 252)` and Bricolage
Grotesque of the live system. That is the divergence that was reported and this
is what closing it looks like.

### Difference classification (§14.1)

| Difference | Class |
|---|---|
| Shell, header, width, card, typography, text colour, motion | IDENTICAL / REUSED — **PASS** |
| Ladder and level-page copy | APPROVED CONTENT — **PASS** |
| "All levels", "Continue with {LEVEL}", ladder → level routing | APPROVED FUNCTION — **PASS** |
| One-column stacking at 834 and 390 | RESPONSIVE — **PASS** |
| Watermarked photograph absent | APPROVED CORRECTION (§5) — **PASS** |
| Endpoint labels absent | APPROVED CORRECTION (§6) — **PASS** |
| Unexplained visual difference | none found |

---

## 7. §7 — the no-clutter gate, reassessed after the rebuild

Density measured as characters of visible text per 1000px of page height, the
same way on both sides.

"Before" is commit `2b7966c`, the candidate you had this morning. "After" is
this one. Both measured the same way on the same machine.

| Surface | Before (`2b7966c`) | After (`665fcb3`) | Live comparison |
|---|---|---|---|
| CEFR ladder `/access/` | 942 | **867** | live `/learner/` 794 |
| Access Panel Home | 981 | **876** | live `/learner/` 794 |
| Access Panel Home + level band | 970 | **872** | live `/learner/` 794 |
| Parents dashboard | 638 | **510** | live parents dashboard 684 |
| Level page `/levels/b2/` | 469 | **506** | live `/learner/app/adult` 569 |
| Level page `/levels/a1/` | — | **505** | live `/learner/app/adult` 569 |

The ladder was the densest learner-facing surface and is not any more. Every
surface except the level pages came down.

The level pages went **up**, 469 to 506, and I would rather say so than leave
you to find it. The reason is that they now carry the system's real header and
footer, which the revoked pages did not have — that is about 65 characters of
chrome on a short page. They remain the least dense pages in the set and sit
below the live adult mini-app landing at 569.

`/preview/lesson-interactions/` measures 1384. That is the interaction preview
harness, which deliberately shows many states of the lesson player on one page
so they can be photographed. It is not a learner route and I have not counted
it as one; the real player is entitlement-gated.

Nothing was deleted to bring these numbers down. All six definitions, all three
band groupings and the placement line are still there. The change is spacing and
arrangement, both taken from the system: the section rhythm is
`py-16 lg:py-20` because that is what `WelcomeSection` uses, and the six levels
sit in the two-column card grid the panel home already uses rather than stacked
on a spine in a narrow right-hand column. No new accordion, carousel or card
system was invented.

---

## 8. §5 — the watermarked image

`public/images/learners.jpg` is deleted. The `<img>` and its gradient overlay
are deleted with it. Nothing replaces it: no other photograph, no AI image, no
placeholder box. The ladder now runs the full page width, which is how the
layout closes the space.

The three band labels that used to sit over the photograph — Basic user,
Independent user, Proficient user — were not lost with it. They are now the
group headings above their own levels, where they read better.

No other image anywhere in the build was touched.

---

## 9. §6 — developer copy on learner surfaces

One switch, `lib/developer-surface.ts`, read by every place that used to print
a route. It is off unless `NEXT_PUBLIC_IELPS_SHOW_ENDPOINTS=1` is set at build
time, so the shipping build carries none of it, and an endpoint-annotated
capture can still be taken for developer evidence.

Removed from view:

- pathway cards on the panel home — `POST /api/auth/register`, `GET /api/assessment/level…`, and the rest
- the panel section header — `GET /api/auth/pathways`
- adult-flow steps — `/dashboard`, `CTA`, `/learner?lesson=:id`, `/api/progress/lesson`
- mini-app panels — including `GET /api/school/parent/dashboard` on the Parent dashboard
- mini-app onboarding steps
- the lesson player's per-step endpoint strip
- the "Server wiring" drawer

No route was renamed, duplicated or removed. No request changed. No backend
behaviour changed. The endpoint data itself (`pathway.backend`, `app.endpoints`,
`panel.endpoint`, `ADULT_FLOW[].route`) is untouched in source and remains the
integration record.

The Live / Empty / Sign in / Unavailable badge stays. It is not an endpoint
label — it states whether what you are reading came from the server, which the
truthfulness rules require.

One thing to be aware of: the lesson player running in production today still
prints `GET /api/curriculum/deep-catalog` at the foot of a lesson step. That is
the live release, which this instruction does not let me touch. The fix is in
this candidate and clears when this candidate is deployed.

---

## 10. §8 — screens not redesigned

Access Panel Home, Parent dashboard, Keyword host page, Reading and Writing
expansion windows and PiP are unchanged except for the §6 label removal, which
the instruction requires. Parent "Needs attention" still displays only
server-supported states.

## 11. §9 — Public Landing

Untouched and visually locked. Verified on the server, not locally:
`~/eilps/frontend/dist` last modified 17 August 12:43, and 0 files changed
under it since 00:00 today.

---

## 12. Administrator deployment pack

I have no privileged access and did not attempt any part of this. It is
written out so it can be executed exactly.

### Current unit, verbatim

    # /etc/systemd/system/eilps-learner.service
    [Unit]
    Description=IELPS immutable A1-C2 learner experience
    After=network.target eilps-web.service
    Wants=eilps-web.service

    [Service]
    Type=simple
    User=anirudhat
    Group=anirudhat
    WorkingDirectory=/home/anirudhat/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery
    Environment=NODE_ENV=production
    Environment=PORT=4302
    Environment=DIST=/home/anirudhat/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery/out
    ExecStart=/usr/bin/node /home/anirudhat/eilps/releases/ielps-a1-c2-learner-20260805-r4-discovery/runtime/learner-server.mjs
    Restart=always
    RestartSec=5

    [Install]
    WantedBy=multi-user.target

### Why the unit must change

The release running today is a static export served by
`runtime/learner-server.mjs` out of a `DIST` directory. This candidate is a
Next.js **standalone** build: it has its own server and does not produce a
static `out/`. The four lines below are the whole difference; nothing else in
the unit changes.

    WorkingDirectory=/home/anirudhat/eilps/releases/<new release dir>
    Environment=DIST=                      ← remove, standalone does not use it
    Environment=HOSTNAME=127.0.0.1         ← add
    ExecStart=/usr/bin/node /home/anirudhat/eilps/releases/<new release dir>/server.js

`PORT=4302`, the user, the ordering and the restart policy all stay as they are.

### Steps

1. Build commit `665fcb3` and place it at
   `~/eilps/releases/ielps-learner-20260818-cefr-conformance/`, including
   `.next/static` copied into `.next/standalone/.next/static` and `public/`
   copied into `.next/standalone/public/` — the standalone server does not
   serve either directory unless they are inside it.
2. Apply the four unit lines above.
3. `systemctl daemon-reload && systemctl restart eilps-learner.service`

### Health checks, through the real addresses

    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/access/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/levels/a1/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/levels/c2/
    curl -o /dev/null -w '%{http_code}\n' https://eilps.com/learner/app/adult
    curl -s https://eilps.com/learner/access/ | grep -c 'Plus_Jakarta\|learners.jpg'

Expected: 200 on the first five, `0` on the last. Check through the public
address, not `127.0.0.1:4302` — that skips the proxy, which is the layer most
likely to need a new location for `/learner/access/`.

Expected interruption: a few seconds on restart.

### Rollback

    # revert the four unit lines to the block above, then
    systemctl daemon-reload && systemctl restart eilps-learner.service

The current release directory is left in place and untouched, so rollback is a
unit revert and a restart. No file in
`ielps-a1-c2-learner-20260805-r4-discovery` is modified by this deployment.

---

## 13. Evidence index

    responsive/   24 whole-page captures — desktop 1280, tablet 834, mobile 390
    pairs/         5 side-by-side comparisons, live baseline left, candidate right
    source-inventory-sha256.txt   125 files, SHA-256 each
    manifest-hash.txt             SHA-256 of that inventory

Pair C is the one to look at first: the live adult mini-app on the left, the
rebuilt CEFR ladder on the right.
