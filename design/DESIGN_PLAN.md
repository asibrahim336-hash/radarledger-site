# DESIGN_PLAN.md — RadarLedger Site

## Subject Grounding

RadarLedger is a **detection instrument for commerce**. It crawls competitor storefronts and ad libraries daily, diffs the results against yesterday, scores every change 0–10, and delivers timestamped evidence to the operator. Its native world is sweeps, signals, timestamps, severity scores, and a ledger of events.

The audience: skeptical, busy DTC founders and marketers who have been burned by dashboards that promise "insights" and deliver noise. They will google this domain after receiving a cold email. The page's single job: make one of them believe the mechanism is real and send an email.

---

## Token System

### Colour Palette (6 values)

| Token | Hex | Role |
|-------|-----|------|
| `--ink` | `#1B2130` | Primary text, headings — deep navy-charcoal |
| `--paper` | `#F4F5F7` | Page background — cool neutral grey |
| `--surface` | `#FFFFFF` | Card/panel backgrounds |
| `--signal` | `#2563EB` | Interactive elements, links, the "signal" colour — electric blue |
| `--alert` | `#DC2626` | High-severity indicators (sev ≥8) |
| `--muted` | `#6B7280` | Secondary text, timestamps, metadata |

**Anti-default check**: Not cream/terracotta (tell #1). Not black+acid-green (tell #2). Not broadsheet hairlines (tell #3). The palette is instrument-grade: cool, precise, with one electric blue as the signal accent. The navy-charcoal ink on cool grey paper reads as technical documentation, not SaaS marketing.

### Typography

| Role | Stack | Weight | Size Scale |
|------|-------|--------|-----------|
| Display (hero only) | `"JetBrains Mono", ui-monospace, monospace` | 700 | 2.25rem → 1.75rem (mobile) |
| Body | `-apple-system, BlinkMacSystemFont, "Segoe UI", Inter, sans-serif` | 400/600 | 1rem base, 1.6 line-height |
| Data/utility | `"JetBrains Mono", ui-monospace, monospace` | 400 | 0.8125rem |

**Rationale**: The hero headline in monospace is the characterful choice — it signals "this is a system that reads data, not a marketing page." Body stays in system sans for readability. Data elements (timestamps, severity scores, event logs) use the same mono, creating visual continuity between the hero and the proof artifacts.

**Font delivery**: Self-host JetBrains Mono Bold (WOFF2 subset: Latin, digits, punctuation, arrows). ~18KB. OFL license committed. Falls back to system monospace if blocked.

### Spacing Scale

Base unit: `0.5rem` (8px). Scale: 0.5 / 1 / 1.5 / 2 / 3 / 4 / 6 / 8 rem.

### Layout

- Max content width: `52rem` (832px) — tighter than typical SaaS; reads like a document
- Single column throughout — no multi-column marketing grids
- Generous vertical rhythm (3–4rem between sections)
- No border-radius on containers (instrument aesthetic); 2px radius on inline chips only

---

## Signature Element: The Detection Ledger Strip

A horizontal strip of **real timestamped events** rendered in monospace with severity chips. This is the hero's proof — not a headline, not a stock photo, but actual output from the system.

```
┌─────────────────────────────────────────────────────────────────────┐
│  2026-07-10 02:36 UTC  ●9  price_drop    €49.00 → €43.12  (−12%)  │
│  2026-07-09 08:14 UTC  ●8  promo_change  "Summer Sale — 20% off"  │
│  2026-07-08 06:02 UTC  ●5  price_up      €19.99 → €20.99  (+5%)   │
└─────────────────────────────────────────────────────────────────────┘
```

- Rendered as a `<pre>` or styled `<ol>` with monospace font
- Severity chips: filled circles coloured by severity (red ≥8, amber 5–7, grey <5)
- Labelled: "Sample output from our control store — we test detection against it daily"
- Scrolls horizontally on mobile (overflow-x: auto) — preserves the "instrument readout" feel
- This IS the hero illustration. No image needed.

---

## ASCII Wireframe

```
┌──────────────────────────────────────────────────────┐
│ [wordmark]                          [Privacy] [Reports] │  ← nav, minimal
├──────────────────────────────────────────────────────┤
│                                                        │
│  HERO: monospace headline                              │
│  "Watches competitor storefronts and Meta ad           │
│   activity daily. Scores every change 0–10.            │
│   Alerts you same-day."                                │
│                                                        │
│  ┌─ DETECTION LEDGER STRIP ──────────────────────┐    │
│  │ timestamp  ●sev  event_type   detail          │    │
│  │ timestamp  ●sev  event_type   detail          │    │
│  │ timestamp  ●sev  event_type   detail          │    │
│  └───────────────────────────────────────────────┘    │
│  caption: "Sample from our control store..."           │
│                                                        │
├──────────────────────────────────────────────────────┤
│  HOW IT WORKS: Watch → Score → Deliver                 │
│  Three short paragraphs, no icons                      │
├──────────────────────────────────────────────────────┤
│  WHAT GETS TRACKED                                     │
│  Two-column list: prices, promos, stock, products,     │
│  ad launches/stops, offer changes                      │
├──────────────────────────────────────────────────────┤
│  BUILT FOR DTC                                         │
│  Verticals + markets in a compact grid                 │
├──────────────────────────────────────────────────────┤
│  EVIDENCE DISCIPLINE                                   │
│  The canary trust story: "We run detection against     │
│  a control store every day and publish nothing we      │
│  can't reproduce."                                     │
│  Measured: alert fires in <1s (SLA: 5 min)             │
├──────────────────────────────────────────────────────┤
│  PILOT CTA                                             │
│  mailto:ahmed@radarledger.com — plain, no form         │
├──────────────────────────────────────────────────────┤
│  FOOTER: Dublin · Privacy · Reports                    │
└──────────────────────────────────────────────────────┘
```

---

## Anti-Default Revision Notes

1. **Rejected**: Warm cream background + serif display → replaced with cool grey + monospace display. The monospace hero is distinctive and grounded in the product's actual output format.
2. **Rejected**: Multi-column feature grid with icons → replaced with single-column document flow. The site reads like a technical brief, not a landing page.
3. **Rejected**: Rounded cards with drop shadows → replaced with flat panels, hairline borders only on the ledger strip. The instrument doesn't decorate.
4. **Kept from T7**: Privacy notice structure (honest, complete). Upgraded: controller placeholder, DRAFT banner.
5. **Kept from T7**: Sample snapshot concept. Upgraded: moved to secondary position; the ledger strip is the hero proof.

---

## Motion

One orchestrated moment: the ledger strip entries fade in sequentially on first viewport intersection (200ms stagger, opacity 0→1 + translateY 4px→0). Respects `prefers-reduced-motion: reduce` — all entries visible immediately with no animation.

---

## Pages

1. **index.html** — Full landing page per wireframe
2. **reports.html** — Three baseline reports listed as "available on request"
3. **privacy.html** — GDPR privacy notice (controller placeholder, DRAFT banner)
4. **404.html** — Minimal "page not found" with nav back to home
