# Municipal Bond Dutch Auction Simulator

An interactive simulation of a **$500,000,000 municipal bond sale priced by single-price ("Dutch") auction** — a three-day order period compressed into 90 seconds, with a live order book, a demand ladder by interest rate, and final allocations.

**Live demo:** https://cupojoseph.github.io/muni-dutch-auction/

Single self-contained `index.html`. No build step, no dependencies, no network calls.

---

## What it demonstrates

Press **Simulate run** and the order period opens. Over 90 seconds of wall clock — mapped to three business days, 9:00 AM to 5:00 PM each — institutional investors submit orders:

- **A required rate and a par amount**, in $100,000 increments. Most orders land in the $5M–$20M range; large funds bid $25M–$75M; SMA desks and community banks bid under $5M.
- Orders arrive **back-loaded**, the way a real order period fills — day 3 carries the heaviest flow.
- Some accounts **revise down** on day three, sharpening their rate to protect an allocation. Revisions supersede the original order and are tagged in the tape.

### How pricing works

1. Eligible orders are stacked from the **lowest rate up** until the target par is filled.
2. The rate that fills the last bond becomes the **clearing rate**.
3. **Every winning investor receives that same clearing rate** — including those who bid well below it. That is the single-price rule, and it is the whole point of the format: bidding aggressively wins you an allocation without costing you yield.
4. Orders **above** the clearing rate get nothing.
5. Orders at exactly the clearing rate are **pro-rated**, rounded down to whole $100,000 lots.

### Reserve rate

The **reserve rate is a ceiling** — the highest coupon the issuer is willing to pay. Bids above it never qualify, and are drawn hatched in red on the ladder. If eligible demand never reaches the target, the sale closes **undersubscribed** and prices at the reserve, selling only what was bid.

Both the target par amount and the reserve rate are editable in the header tiles while the auction is idle.

---

## Reading the interface

Every tile, panel, legend key and control carries a small **`i` marker** — hover, focus or tap it for an explanation of what that element means and how that part of the auction works.

| Panel | What it shows |
| --- | --- |
| **Demand ladder** | The core visual: a cumulative demand curve drawn as a waterfall. Each row is a 0.05% rate bucket — the solid blue segment is *new* demand at that rate, the pale segment behind it is everything committed at or below. Both are measured along one shared axis, so the dashed $500M mark is a single point on that axis (the moment demand covers the deal), not a threshold each rate has to hit on its own. A blue marker pins where the curve actually crosses. |
| **Rate-level hover** | Hover any ladder row — including while the auction is running — for the orders behind it: new demand at that rate, cumulative to that point, share of target, whether it fills, and the individual investors with their sizes. |
| **Live order flow** | Every order as it lands: who bid, what type of account, par amount, rate. Revisions and above-reserve bids are flagged. |
| **Capital committed vs. target** | Eligible demand against the target, with the oversubscribed excess hatched beyond the notch. |
| **Clearing rate** | Updates live as the book builds. Reads *Not covered* until eligible demand first reaches the target, then tightens downward as aggressive orders arrive. |
| **Allocations** | Final pricing: par sold, annual interest cost, savings against the reserve, pro-rata factor at the clearing tier, and a per-investor fill table. |
| **Book by investor** | Top 12 accounts by aggregate par, updating live. |

### Colour encodings

Every row of the ladder is drawn as **two bars end to end**, both measured along the same cumulative axis:

| Colour | Meaning |
| --- | --- |
| **Pale blue** | The running total already committed at *cheaper* rates, before this row adds anything. This is why bars start further right as you move down — each row inherits the total above it. |
| **Dark blue** | The new money arriving at *this* rate. Its length is the figure printed at the end of the row, and these segments stack into the staircase. |
| **Grey** | Eligible demand that was outbid — the target was already covered by cheaper money before the stack reached it, so it fills nothing. |
| **Red hatch** | Above the reserve, disqualified outright. Never enters the cumulative stack, which is why the pale bar stops growing below these rows. |
| **Blue marker** | Where the cumulative curve crosses the target. Its row is the clearing rate. |

In the order flow, the **rate itself is colour-coded**: blue is the order as first submitted, green means the account revised down to a sharper rate (the revision replaces the original rather than adding to it).

**Controls:** Simulate run · Pause/Resume · speed 1×/2×/4× · Reset · light/dark toggle.

---

## Disclaimer

Demonstration software. The offering, the CUSIP, every investor name, order size and rate are **fabricated**. Recognizable firms — BlackRock, Fidelity, Vanguard, PIMCO, Nuveen, Goldman Sachs Asset Management and others — appear as stand-ins for institutional buyer types to make the demo legible; they did not participate in anything and none of this reflects their actual behavior. Nothing here is an offer, a solicitation, or investment advice.

## License

MIT
