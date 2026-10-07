# PerkKeeper catalog

Card benefits for the PerkKeeper family app. The app downloads `catalog.json` from
`main` (at most every 12 hours) and applies it when `version` is newer than the copy it has.

Contains only public card information — no names, holders, card numbers or spending.

## Updating

Changes come in as pull requests (usually from the scheduled research job) and go live when merged.

Rules for every change:
- Bump `version` to the date of the change (`YYYY-MM-DD`; same-day reruns: `YYYY-MM-DD.2`). The app ignores a catalog whose version isn't newer.
- Never change a card `id` or benefit `id` — the app uses them to match existing data and history.
- Remove a credit by deleting it; the app retires it and keeps its history.
- Rotating quarterly rates get `"until": "<first day of next quarter>"`.
- Cite a source (issuer page preferred) for every change in the PR description.

## Format

```jsonc
{
  "version": "2026-10-07",
  "cards": [{
    "id": "chase-sapphire-preferred",      // stable, never change
    "name": "Chase Sapphire Preferred",
    "issuer": "Chase",
    "kind": "credit",                       // "credit" (default) or "hsa"
    "annualFee": 95,
    "baseMultiplier": 1,                    // everything else; 0 for store-only cards
    "rotating": false,                      // quarterly 5% categories that need activation
    "hsaLimit": 8750,                       // HSA only
    "notes": "…",
    "rates": [
      { "category": "dining", "multiplier": 3, "note": "…" },
      { "merchant": "Costco", "multiplier": 2 },
      { "category": "groceries", "multiplier": 5, "until": "2027-01-01", "note": "Q4 bonus" }
    ],
    "benefits": [
      { "id": "csp-hotel-credit", "name": "…", "amount": 100,
        "period": "monthly|quarterly|semiannual|annual|cardYear", "category": "hotels", "notes": "…" }
    ]
  }]
}
```

Categories: dining, groceries, gas, travel, flights, hotels, transit, rideshare, streaming,
drugstore, medical, online, everythingElse.
