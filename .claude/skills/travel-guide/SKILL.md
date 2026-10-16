---
name: travel-guide
description: Personalized credit card points and airline miles strategy advisor. This skill should be used when the user asks how to use credit card points efficiently, when to transfer points to airlines or hotels, which airline or alliance to focus on, how to consolidate frequent-flyer accounts, or which transfer partners to use. The skill asks for the user's usual airline and credit cards, then recommends which alliance to anchor on and how to earn and redeem.
---

# Travel Guide: Credit Card Points & Miles Strategy

## Purpose

Give personalized, correct guidance on maximizing credit card points and airline miles.
The advice depends on two facts about the user: their usual airline (which determines
their home alliance) and which credit cards they hold (which determines their transfer
partners). Always gather those two facts first, then tailor the recommendation.

## Workflow

### Step 1: Gather the two personalizing facts

Before giving any advice, ask the user (use the AskUserQuestion tool when available, so
answers are structured):

1. **"Which airline do you usually fly, or which airport are you based out of?"**
   This determines the home alliance to anchor on.
2. **"Which credit cards do you have?"** (e.g., Amex Platinum, Chase Sapphire Reserve,
   Capital One Venture, Citi Premier.) This determines which transfer partners are reachable.

If the user already stated these in the conversation, skip the question and use what they said.

### Step 2: Map airline to alliance

Use `references/alliances.md` to identify the user's home alliance and primary program.
Anchor recommendation: pick the one program in that alliance whose airline has the most
nonstops from the user's home airport.

### Step 3: Map credit cards to transfer partners

Use `references/transfer-partners.md` to determine which of the user's home-alliance
programs their cards can transfer to directly, and which require partner backdoors.

### Step 4: Deliver tailored advice

Apply the core rules in `references/core-strategy.md`, personalized to the user's alliance
and cards. Cover, as relevant to the question:

- When to transfer (only when booking; the exceptions).
- Which one program to anchor on and why.
- How to earn: put the anchor program's number on every flight in that alliance; check a
  "where to credit" calculator on deep-discount fares.
- How to redeem: book the flight through whichever alliance program prices the award
  cheapest, funded by the matching card's points.
- Hotels: open an account per brand stayed at, concentrate stays in 1-2 chains for status.
- Consolidation: miles cannot be merged across airlines; consolidate future earning, spend
  down or top off orphan balances.

## Reference files

- `references/core-strategy.md` — the timing rules, earning/redeeming logic, hotels,
  consolidation, and anticipated follow-up questions.
- `references/alliances.md` — what an alliance is, the three alliances and their members,
  and how to pick a primary program by home airport.
- `references/transfer-partners.md` — which cards (Amex MR, Chase UR, etc.) transfer to
  which airline/hotel programs, and the best sweet spots.

Load the reference file relevant to the user's question rather than all three at once.
