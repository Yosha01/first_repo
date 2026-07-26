<!--
REPORT_TEMPLATE.md
Blueprint for the daily pre-market report. This is the single source of truth for
structure. The analyst prompts (Claude pass, Codex pass) and the merge prompt all
get written against this file, so section order, table columns and the conviction
key do not drift.

Rules for whoever fills it in:
- Keep the sections in this exact order. Do not add or drop sections.
- Every {{PLACEHOLDER}} gets replaced. If there is genuinely nothing for a slot,
  write "nothing worth flagging" instead of leaving it blank or padding it.
- Voice is casual trader talk, plain and a little blunt. Risk first, hype never.
- No em dashes anywhere in the output.
- Numbers get a source and a timestamp. If a number is stale, say it is stale.
- The rules engine picks who makes the watchlist. The two AI passes only judge
  setup quality. Neither AI adds a ticker the scan did not surface.
-->

# Pre-Market Report

*{{WEEKDAY}}, {{MONTH}} {{DD}}, {{YYYY}} | Prepared before the open. Claude and Codex each ran the tape independently, no peeking, then the two passes were merged.*

<!-- Subtitle rule: always name both models and always say the passes were independent.
     Add the scan cutoff time if it matters, e.g. "scan cutoff 8:15am ET". -->

> The scanner rules pick the watchlist, Claude and Codex only grade the setups, and none of this is financial advice.

<!-- Disclaimer: exactly one line, every day, no expanding it into a paragraph. -->

## Summary

- **The tape:** {{ONE LINE ON WHAT THE MARKET IS DOING PREMARKET. Index futures, direction, whether it is a real move or chop, one number to anchor it.}}
- **The catch we are watching:** {{ONE LINE ON THE THING THAT COULD BREAK THE DAY. The event, the crowded trade, the gap that smells wrong, the level that has to hold.}}
- **Two-brain verdict:** {{ONE LINE. Where Claude and Codex landed together and how much size that view earns. Example shape: "Both passes land on selective and small, one clean A setup, everything else is a maybe."}}

## Pre-Market Gappers

<!-- Every gapper that cleared the scan, sorted by gap percent, biggest first.
     The catalyst headline goes in verbatim, full headline, not a paraphrase. If there
     is no headline, write "No headline found" and treat that as a warning sign, not a
     detail. Cap the list at the top {{N}} unless something unusual is running. -->

- **{{TICKER}}** {{+/-XX.X}}% at ${{PRICE}}, {{PREMARKET VOLUME}} shares premarket, float {{FLOAT}}, avg daily vol {{ADV}}
  - Catalyst: "{{FULL CATALYST HEADLINE, VERBATIM}}" ({{SOURCE}}, {{TIME ET}})
  - Read: {{ONE OR TWO LINES. Is the catalyst real, is the float tight, is the volume backing the move or is it three people trading it.}}

- **{{TICKER}}** {{+/-XX.X}}% at ${{PRICE}}, {{PREMARKET VOLUME}} shares premarket, float {{FLOAT}}, avg daily vol {{ADV}}
  - Catalyst: "{{FULL CATALYST HEADLINE, VERBATIM}}" ({{SOURCE}}, {{TIME ET}})
  - Read: {{ONE OR TWO LINES.}}

- **{{TICKER}}** {{+/-XX.X}}% at ${{PRICE}}, {{PREMARKET VOLUME}} shares premarket, float {{FLOAT}}, avg daily vol {{ADV}}
  - Catalyst: "{{FULL CATALYST HEADLINE, VERBATIM}}" ({{SOURCE}}, {{TIME ET}})
  - Read: {{ONE OR TWO LINES.}}

**Conviction key:** 🟢 green, both brains like it and the levels are clean, size normal | 🟡 yellow, setup is there but something is off, half size or wait for confirmation | 🔴 red, on the list for awareness only, do not touch it without a change in the tape

<!-- The key gets printed once, right here, and it governs both watchlist tables below. -->

## Day Trading Watchlist

<!-- Intraday only, flat by the close. Levels are actual prices, never "resistance above".
     Plan states the trigger, the stop and the first target. Codex check is what the
     second pass said about this exact name, including when it disagreed. -->

| Ticker | Catalyst | Levels | Plan | Codex check | Conviction |
| --- | --- | --- | --- | --- | --- |
| {{TICKER}} | {{SHORT CATALYST, ONE CLAUSE}} | Support {{$X}}, resistance {{$Y}}, premarket high {{$Z}} | {{TRIGGER, STOP, FIRST TARGET}} | {{WHAT CODEX SAID, AGREE OR DISAGREE AND WHY}} | {{🟢/🟡/🔴}} |
| {{TICKER}} | {{SHORT CATALYST}} | Support {{$X}}, resistance {{$Y}}, VWAP {{$W}} | {{TRIGGER, STOP, FIRST TARGET}} | {{WHAT CODEX SAID}} | {{🟢/🟡/🔴}} |
| {{TICKER}} | {{SHORT CATALYST}} | {{KEY LEVELS}} | {{TRIGGER, STOP, FIRST TARGET}} | {{WHAT CODEX SAID}} | {{🟢/🟡/🔴}} |

## Swing Watchlist

<!-- Multi day holds. Trend context is the higher timeframe picture, what the daily and
     weekly are doing, where price sits against the 20, 50 and 200. Idea is the thesis
     plus the invalidation. Same Codex check and same conviction key. -->

| Ticker | Catalyst | Trend context | Idea | Codex check | Conviction |
| --- | --- | --- | --- | --- | --- |
| {{TICKER}} | {{CATALYST OR THEME}} | {{DAILY AND WEEKLY STRUCTURE, POSITION VS 20/50/200 MA}} | {{THESIS, ENTRY ZONE, INVALIDATION, TARGET}} | {{WHAT CODEX SAID}} | {{🟢/🟡/🔴}} |
| {{TICKER}} | {{CATALYST OR THEME}} | {{TREND CONTEXT}} | {{THESIS, ENTRY ZONE, INVALIDATION, TARGET}} | {{WHAT CODEX SAID}} | {{🟢/🟡/🔴}} |
| {{TICKER}} | {{CATALYST OR THEME}} | {{TREND CONTEXT}} | {{THESIS, ENTRY ZONE, INVALIDATION, TARGET}} | {{WHAT CODEX SAID}} | {{🟢/🟡/🔴}} |

## Market Trends of the Day

<!-- Three to five bullets. Sectors, rotation, what is actually leading and what is
     quietly bleeding. Name the ETF or the group, not just the vibe. Tie it back to the
     watchlist where it connects. -->

- **{{SECTOR OR THEME}}:** {{WHAT IS HAPPENING AND WHAT IT MEANS FOR THE NAMES ABOVE}}
- **{{SECTOR OR THEME}}:** {{...}}
- **{{SECTOR OR THEME}}:** {{...}}
- **Under the hood:** {{BREADTH, VOLUME, ANY DIVERGENCE BETWEEN THE INDEX AND THE AVERAGE STOCK}}

## Technical Signals for Today

<!-- Index and market wide technicals, not single names. Levels that matter for the
     whole session. Keep it to what changes behavior, skip the indicator soup. -->

- **{{INDEX OR ETF, e.g. SPY}}:** {{KEY SUPPORT AND RESISTANCE, WHAT A BREAK OF EACH MEANS}}
- **{{INDEX OR ETF, e.g. QQQ}}:** {{KEY LEVELS AND STRUCTURE}}
- **Volatility:** {{VIX LEVEL, DIRECTION, WHAT IT IMPLIES FOR SIZE TODAY}}
- **Signals firing:** {{NOTABLE CROSSES, GAPS TO FILL, RANGE BREAKS, FAILED BREAKDOWNS}}
- **What invalidates the day:** {{THE ONE LEVEL THAT, IF LOST OR RECLAIMED, MEANS SIT OUT OR FLIP THE BIAS}}

## Economic Data, Rates and the Fed

<!-- Pull from the econ calendar. Only today's releases plus anything overnight that
     already moved the tape. Every row gets time, consensus and prior. After the table,
     one short read on rates and Fed positioning. -->

| Time (ET) | Release | Consensus | Prior | Why it matters |
| --- | --- | --- | --- | --- |
| {{HH:MM}} | {{RELEASE NAME}} | {{CONSENSUS}} | {{PRIOR}} | {{ONE CLAUSE}} |
| {{HH:MM}} | {{RELEASE NAME}} | {{CONSENSUS}} | {{PRIOR}} | {{ONE CLAUSE}} |

- **Rates:** {{2Y AND 10Y LEVELS AND DIRECTION, CURVE NOTE IF RELEVANT}}
- **Fed:** {{SPEAKERS TODAY, LATEST DOT PLOT OR MINUTES TAKEAWAY, CURRENT MARKET IMPLIED ODDS FOR THE NEXT MEETING}}
- **Read:** {{ONE OR TWO LINES ON HOW THIS SHOULD CHANGE SIZING AND TIMING TODAY, INCLUDING WHICH RELEASE IS WORTH STANDING ASIDE FOR}}

## Coming Up

<!-- Tomorrow only, plus anything later this week big enough to already be shaping
     positioning. Split events from earnings. Mark before open and after close. -->

**Events and data**
- {{DATE, TIME ET}}: {{EVENT OR RELEASE}} {{ONE CLAUSE ON WHY IT MATTERS}}
- {{DATE, TIME ET}}: {{EVENT OR RELEASE}}

**Earnings**
- {{TICKER}} ({{BEFORE OPEN / AFTER CLOSE}}): {{WHAT THE STREET EXPECTS, IMPLIED MOVE IF AVAILABLE, WHY IT MATTERS BEYOND THAT NAME}}
- {{TICKER}} ({{BEFORE OPEN / AFTER CLOSE}}): {{...}}

**Positions to think about now:** {{ANYTHING ON THE WATCHLISTS ABOVE THAT CARRIES EVENT RISK INTO TOMORROW}}

## Skips and Traps

<!-- The names that look great and are not. Dilution risk, offering language in the
     filing, low float pump, gap with no volume, illiquid options, extended into
     resistance, headline already priced in. One line each, be specific about the tell. -->

- **{{TICKER}}:** {{WHAT IT LOOKS LIKE, THEN THE TELL THAT KILLS IT}}
- **{{TICKER}}:** {{...}}
- **{{TICKER}}:** {{...}}
- **General trap today:** {{THE BEHAVIORAL ONE. Chasing the open, revenge trading the gap fill, sizing up into a data release, whatever fits the tape.}}

## Where the two brains landed

<!-- Written by the merge pass, never by either analyst pass. Honest about disagreement.
     If both passes agreed on everything, say so plainly and treat it as a mild warning
     that they may be reading the same surface data. -->

**Agreement:** {{WHAT BOTH PASSES INDEPENDENTLY LANDED ON. Names, direction, sizing posture. Be specific, "both were cautious" is not an answer.}}

**Rules versus discretion:** {{WHERE THE SCAN OUTPUT AND THE AI JUDGMENT PULLED APART. Which tickers the rules surfaced that both brains graded down, which ones the rules ranked low but the setups actually looked clean, and which side got the benefit of the doubt today.}}

**Claude's sharp catch:** {{THE ONE THING THIS PASS SAW THAT THE OTHER MISSED. One specific observation, not a summary.}}

**Codex's sharp catch:** {{THE ONE THING THAT PASS SAW THAT THE OTHER MISSED. One specific observation, not a summary.}}

**Net:** {{ONE LINE. What a trader should actually do with all of this at 9:30.}}
