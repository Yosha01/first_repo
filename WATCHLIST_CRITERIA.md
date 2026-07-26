# Watchlist Criteria

Source of truth for the scanner. Two validated setups, one for day trading, one for swing. If the scanner and this file disagree, this file wins and the scanner gets fixed.

Read the comparisons literally. `>` means strictly greater, `>=` means greater or equal. The day gap is `> 3%` and the swing gap is `>= 8%`. That is on purpose, do not round them into each other.

All times are ET.

---

## Day Trading Watchlist: Trend Join Long

**Backtest:** 54.6% win rate, profit factor 1.59, 280 trades.

Not a big edge per trade, it is a grind. It works because the plan is boring and the stop is tight. Skip a name the second it stops fitting.

### Premarket screen, all five required

| # | Rule | How to compute |
| --- | --- | --- |
| 1 | Gap % vs prev close > 3% | `(premarket price / prev regular session close - 1) * 100` |
| 2 | Price > $3 | Current premarket price |
| 3 | Market cap > $1B | Shares outstanding x current premarket price |
| 4 | Premarket RVOL > 1.5 | Premarket volume so far vs the average premarket volume at this same clock time |
| 5 | Price breaking above yesterday's high | Premarket price > yesterday's regular session high |

Fail any one of the five and the name is off the list. There is no "close enough" and no override for a good story.

### Intraday plan

- **Window:** entries only between 10:00am and 3:30pm. Nothing in the first thirty minutes, that opening range is not this setup.
- **Trigger:** price > premarket high AND price > prior high of day. Both, at the same time. Prior high of day means the session high built since 9:30, before the trigger bar.
- **Stop:** 1% below whichever is lower, the premarket high or the low of day. That distance is 1R.
- **Targets:** scale 1/3 at +1R, scale 1/3 at +2R, trail the last 1/3 on the 21 EMA.
- **Hard out:** flat by 3:51pm. No exceptions, no holding the runner into the close because it feels strong.

### What kills the trade

- No trigger by 3:30pm. The setup expired, leave it alone.
- Trigger fires but the stop is already more than {{MAX_R_PERCENT}}% away. Risk is too wide, skip it rather than shrinking the size to nothing.
- Halted on the gap. Wait for it to reopen and re-check the levels before touching it.

---

## Swing Watchlist

**Backtest, split by catalyst type:**

| Catalyst | Win rate | Profit factor |
| --- | --- | --- |
| News, no earnings | 57.6% | 5.34 |
| Earnings on the gap day | 44.7% | 2.57 |

Both buckets are worth trading, but they are not the same trade. The news bucket wins more often and pays a lot better. Earnings gaps win less than half the time and still make money on the tail. Tag every swing candidate with its bucket so the size decision is honest about which one it is.

### Premarket screen, all six required

| # | Rule | How to compute |
| --- | --- | --- |
| 1 | Gap % >= 8% | `(open / prev regular session close - 1) * 100` |
| 2 | Price > $3 | Current price |
| 3 | Open > yesterday's high | Regular session open vs yesterday's regular session high |
| 4 | Open > 200 day SMA | Regular session open vs the 200 day simple moving average of daily closes |
| 5 | Market cap >= $800M | Shares outstanding x price |
| 6 | Real catalyst | Earnings released on the gap day, or news with no earnings attached |

**On the catalyst rule:** a real catalyst is a specific, dated, sourced event. Earnings on the gap day, or news on the gap day with no earnings. A generic "shares are moving" wire story is not a catalyst, and neither is an analyst note nobody read. If there is no headline to quote, the name does not qualify.

### Timing gotcha, read this before encoding

Rules 1, 3 and 4 all reference the **open**, which does not exist until 9:30. This list cannot be finalized premarket. Run it in two stages:

1. **Premarket, provisional:** substitute the current premarket price for the open and build the candidate list.
2. **9:30 to 9:35, confirmed:** recompute rules 1, 3 and 4 against the actual open. Anything that fails drops off. Anything that only qualifies after the real print gets added.

The report goes out before the confirm pass runs, so premarket swing names ship as provisional and get marked that way.

### Entry and exit

Still being built. Until it is done:

- Swing names are **starter ideas only**. Name, catalyst, bucket, trend context. That is it.
- **No invented stops, no invented targets, no invented entry zones.** A made up level that looks precise is worse than saying nothing, because it gets traded.
- If a template or a prompt asks for a swing stop or target, the correct answer is "entry and exit management not built yet", not a plausible looking number.

---

## Things I still need to pin down

These are genuinely ambiguous, and the scanner will make a silent choice if nobody makes a loud one.

1. **RVOL baseline.** Average premarket volume over how many days, and is it a time of day cumulative comparison or a flat daily average? A flat average makes an 8:00am scan look weak and a 9:25am scan look strong.
2. **21 EMA timeframe.** 1 minute, 5 minute or 15 minute changes the trail a lot. The backtest used one of them, that is the one to encode.
3. **Stop wording.** I read "1% below premarket high or LOD, whichever is lower" as: take the lower of the two levels, then drop 1% below it. The other reading is min(PMH minus 1%, LOD). Same thing most days, different on a gap that faded hard.
4. **Low of day timing.** LOD at the moment of entry, or LOD updated as the trade runs? Assumed frozen at entry so 1R does not move under the position.
5. **Market cap timing.** Cap computed off the gapped premarket price or off the prev close? A 40% gapper can cross the $1B line on the gap alone.
6. **`{{MAX_R_PERCENT}}`** above. Needs a real number or the rule gets deleted.

---

## How this feeds the report

- The day list fills the **Day Trading Watchlist** table in `REPORT_TEMPLATE.md`. All five screen rules are already met by anything on it, so the Levels and Plan columns come straight from the intraday plan above.
- The swing list fills the **Swing Watchlist** table, with the Idea column carrying the thesis and the catalyst bucket, and no fabricated levels.
- The scanner picks who is on these lists. The Claude and Codex passes only grade what the rules surfaced. Neither one adds a name that failed a screen, and neither one removes a name it just does not like. Grading it 🔴 is how a brain says no.
