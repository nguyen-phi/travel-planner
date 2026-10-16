# Glossary

Canonical domain terms for the travel-planner project. Keep this free of implementation detail.

## Travel Event

One end-to-end trip: a bounded journey with an origin, one or more destinations, and dates, comprising its flights, lodging, and ground transport. The unit the Harness plans. A single bookable component (just a flight) or a multi-trip yearly portfolio is **not** a Travel Event.

## Harness

The system that plans a Travel Event. Currently a suite of agent skills that orchestrate the planning. A future goal is to transform it into a standalone application (see the a→b evolution noted in the Destination); that goal is a standing design constraint now, not current scope.

## Plan

To produce a recommended itinerary and points-redemption strategy for a Travel Event, ending in a hand-off checklist the human executes. The Harness recommends; it does **not** book, move money, or transfer points. Taking booking actions is a separate future effort, out of scope for this map. As a noun, a **Plan** is the markdown document the Harness produces.

## Canonical Profile

The single stored record of the traveler's standing facts: card portfolio, points balances, home airports, airline anchors, and lodging preferences. It lives in exactly one location and is the sole source every skill and computation reads. Skills like `travel-guide` and `card-benefits` feed it; none keep their own copy.

## Option

A discrete choice the Harness surfaces at a decision step (a flight routing, a lodging pick, a points-vs-cash path). Planning proceeds step by step: at each step the Harness presents multiple Options with their tradeoffs, the human picks one, and the accumulated picks compose the Plan.
