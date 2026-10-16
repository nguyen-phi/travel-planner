---
name: card-benefits
description: Show a cached comparison chart of credit card benefits (Chase Sapphire Reserve, Amex Platinum, Capital One Venture, Chase Freedom Flex). Use when the user asks to see, compare, or look up their card benefits, credits, earning rates, lounge access, or annual fees. The chart is cached; only rebuild it from live sources when the user passes "refresh".
---

# Card Benefits Chart

## Purpose

Maintain a cached benefits comparison chart for the user's credit cards and show it on
demand without re-fetching every time. Refresh from live sources only when the user asks.

Tracked cards: Chase Sapphire Reserve, Amex Platinum, Capital One Venture, Chase Freedom Flex.

## Cache file

`references/benefits-chart.md` holds the chart and a `Last refreshed:` date at the top.

## Behavior

Check the skill's argument.

### Default (no argument, or any argument other than a refresh trigger)

Read `references/benefits-chart.md` and present the chart to the user. **Do not fetch the
web.** After showing it, note the `Last refreshed:` date and remind the user they can run
`refresh` to update it.

If the cache file is missing, tell the user and offer to build it (treat as a refresh).

### Refresh (argument is `refresh`, `/refresh`, `update`, or `rebuild`)

1. For each tracked card, search the web for current benefits. Use focused queries per card,
   for example: `<card name> benefits <current year> annual fee credits earning rates`.
2. Prefer primary sources (issuer pages: chase.com, americanexpress.com, capitalone.com)
   and cross-check with reputable points sites (Upgraded Points, NerdWallet, The Points Guy,
   Frequent Miler) for anything the issuer page omits.
3. Capture, per card: annual fee, points currency, earning rates, annual/statement credits,
   lounge access, hotel elite status, transfer partners, and best redemption use.
4. Rewrite `references/benefits-chart.md` in the same structure as the existing file:
   at-a-glance table, earning rates, credits, lounges/status, points value, and dated
   freshness notes with source links.
5. Update the `Last refreshed:` date to today.
6. Show the rebuilt chart and confirm what changed since the previous version.

## Adding or removing cards

If the user wants to track a different set of cards, update the card list in this file and in
the cache file's header, then run a refresh.
