# Award-Data Source Landscape

Research date: 2026-10-15. Resolves wayfinder ticket [#2](https://github.com/nguyen-phi/travel-planner/issues/2). Prices and terms change often; reverify before relying on any single source.

Question: What data sources can the Harness use to inform award-flight and cash recommendations, and on what terms? Is "advisory-first" the right v1 posture, and what would a later live-data integration require?

---

## Verdict (read this first)

- **Advisory-first is the right v1 posture.** For the skill-suite phase (single user, non-commercial), the Harness should reason from the Canonical Profile + strategy skills and **output specific queries for the human to run**. No source offers a free, commercial-clean, reliable availability feed, so hard-wiring live data now buys fragility and legal exposure for little gain.
- **If you want live data in v1 anyway, one personal subscription unlocks it cleanly: seats.aero Pro ($9.99/mo).** It publishes a documented Partner API, grants Pro users 1,000 calls/day at no extra cost, covers ~20 mileage programs, and is explicitly agent/MCP-friendly. **Catch: this is non-commercial personal use only.**
- **The future standalone app (the a→b transform) cannot ride the personal tier.** seats.aero's terms forbid commercial API use without **written permission and unpublished (negotiated) pricing**. A commercial product needs a written agreement with seats.aero or PointsYeah, or an enterprise provider (AwardTool, Award Flight Daily). This is a real constraint for ticket [#9](https://github.com/nguyen-phi/travel-planner/issues/9).
- **Direct scraping of program sites (Avios, LifeMiles, Flying Blue, Hyatt) is the wrong path.** No official public availability APIs exist; scraping violates most airline terms, breaks often, and Hyatt's own award pages already block automated fetching (HTTP 403, confirmed in `hotel-and-alternative-redemptions.md`). Use aggregators, not direct scrapes.
- **point.me is a consumer UI, not a data source.** No public API. Good for the human to run as an advisory target, not for the Harness to call.

---

## Source-by-source

### Aggregators with an API (candidate live-data backends)

| Source | API? | Personal price | API limit | Programs | Commercial use |
|---|---|---|---|---|---|
| **seats.aero** | Yes, documented Partner API | Pro **$9.99/mo** / $99.99/yr | **1,000 calls/day** free for Pro | ~20 mileage programs | **Written permission required**; contact support@seats.aero, negotiated pricing |
| **PointsYeah** | Yes, documented for Premium | **$11.99/mo** / $99.99/yr | 1,000 flight-search calls/day | 20+ programs, incl. hotels | Unclear; developer page exists |
| **AwardTool** | Enterprise API | Enterprise (unpublished) | — | 28 loyalty programs; award hotel calendars + seat maps | Enterprise-oriented |
| **Award Flight Daily** | Agent-native API | Unpublished | — | 25 programs, 12M+ award records | Built for AI agents/apps |

**seats.aero detail:** Authenticate with a `Partner-Authorization` header. Endpoints include Cached Search, Bulk Availability, Get Trips, Get Routes; **Live Search is excluded from the Pro tier.** The API returns award availability, mileage pricing, and cash fares across ~70,000 routes. A separate Rooms.aero hotel API gives another 1,000 calls/day. A community MCP server wraps the Partner API (updated Aug 2026), so wiring it into an agent is low-effort. Terms: **non-automated** access is fine for commercial or personal use without permission; **automated (API)** access for a commercial purpose requires written company permission.

### Consumer tools (advisory targets, not backends)

| Source | API? | Price | Notes |
|---|---|---|---|
| **point.me** | No public API | Free (60-day, 15 results); Standard $12/mo or $129/yr; Premium $260/yr | Live Search + booking instructions; human Concierge from $200/pax. Best used as a "go run this search" target. |

### Program sites (do not scrape)

Avianca LifeMiles, British Airways / Iberia Avios, Air France-KLM Flying Blue, and World of Hyatt expose **no official public availability API**. Third-party scrapers exist (Apify "Flightpoints Award Flights Scraper," others) but automated access generally violates program terms and is brittle. Hyatt's award pages return HTTP 403 to automated fetching. Treat these as human-run advisory targets or reach them indirectly through an aggregator that already licenses the data.

---

## What a later live-data integration requires (for ticket #9)

1. **Single-user skill phase (now):** a personal **seats.aero Pro** key ($9.99/mo) behind an MCP server, or **PointsYeah Premium** ($11.99/mo). Either gives ~1,000 calls/day — ample for one planner. Keep the data-source call behind a clean seam so the Harness works identically whether the seam returns live data or an "ask the human to run this query" stub.
2. **Commercial app phase (the a→b transform):** a **written commercial agreement** with seats.aero (`support@seats.aero`, company email, use case) at negotiated pricing, or an enterprise provider (AwardTool, Award Flight Daily). Budget for per-call or subscription cost and a terms-of-service review. The personal tier does **not** transfer to a product.

**Design implication:** define the award-data access as a single module interface (input: route + dates + programs; output: availability + mileage/cash price, or a human-query stub). v1 implements the stub; the paid API is a drop-in replacement behind the same interface. This keeps advisory-first and a future live feed on the same seam, and keeps the commercial-licensing decision isolated to one place.

---

## Sources

- [seats.aero Partner API — Getting Started](https://developers.seats.aero/reference/getting-started-p)
- [seats.aero Pro API: Access, Limits, and Usage (Knowledge Base)](https://docs.seats.aero/article/68-seatsaero-pro-api-access-limits-and-usage)
- [seats.aero — Terms and Conditions](https://seats.aero/terms)
- [Seats.aero Free vs. Pro — UpgradedPoints](https://upgradedpoints.com/news/seats-aero-free-vs-pro/)
- [Finding award availability with Claude Code & seats.aero (2026)](https://meilen-koenig.de/en/blog/praemienflug-verfuegbarkeit-finden-claude-code-seats-aero)
- [point.me Review 2026 — The Points Party](https://thepointsparty.com/articles/point-me-review)
- [point.me Review — NerdWallet](https://www.nerdwallet.com/travel/learn/point-me-award-search-review)
- [PointsYeah API Documentation](https://www.pointsyeah.com/developers)
- [PointsYeah Review — NerdWallet](https://www.nerdwallet.com/travel/learn/points-yeah-award-search-review-easily-find-your-next-points-redemption)
- [AwardTool Enterprise API](https://www.awardtool.com/enterprise-api)
- [Award Flight Daily — AI API for Award Travel](https://mcpmarket.com/server/award-flight-daily)
- [Flightpoints Award Flights Scraper (Apify)](https://apify.com/automation-lab/flightpoints-award-flights-scraper/api)
