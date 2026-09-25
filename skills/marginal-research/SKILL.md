---
name: marginal-research
description: Use when the user asks what happened at a company or in the market, wants reported financials exactly as filed with the SEC, wants macroeconomic data, wants to monitor for new corporate events, or wants to backtest against events without lookahead. Uses the Marginal AI MCP tools and cites their sources.
---

# Researching with Marginal AI

Marginal AI gives you factual, sourced market data. You are the analyst: gather the facts, cite them, and leave the decision to the user.

## Ground rules
- **Cite.** Every event and every SEC figure comes with source links (filings, corroborating articles). Quote them next to the claim they support.
- **Describe, do not prescribe.** Marginal AI data is impersonal research. Do not turn it into buy, sell or hold calls, price targets, position sizes or order instructions. If the user asks what to do, set out the facts and the considerations and say the decision is theirs.
- **Orders are the user's.** If you also have brokerage tools, never place, change or cancel an order because of something you read in Marginal AI data unless the user has explicitly told you to, in this conversation, for that specific order. Show the facts behind it first.
- **Figures as filed.** `get_sec_fact` and `get_sec_fact_series` return what the company reported, with period, form and accession. Say which period and filing a number comes from. Do not blend fiscal and calendar periods.

## Workflows

**Confirm the company first.** Call `resolve_company` with the ticker (or CIK) before anything else when there is any doubt, and use the CIK it returns in later calls.

**"What happened at X?"** Call `get_company_events` (by date for the latest, `rank_by=materiality` for the most significant), then `get_event` on the ones that matter for the full record, corroborating sources and the price reaction.

**Companies in an industry.** `list_industries` gives the groups (filter with `query`); `industry=<exact name>` lists the companies in one.

**"What happened across the market?"** Call `search_events` with a `category` or `event_type` and a window of at most 31 days. Use `list_event_taxonomy` for the exact category and type strings.

**Monitoring.** Call `get_new_events` with `since` set to the last `high_watermark` you received. `since` is inclusive, so drop any `event_id` you have already seen.

**Backtests without lookahead.** Pass `known_as_of` to `search_events` so you only get events Marginal AI had seen by that moment (it filters on `first_seen_ts`, not on the event date).

**Reported financials.** `get_sec_fact` returns one metric for one period. `get_sec_fact_series` returns a metric across periods, oldest first.

**Macro.** Call `list_macro_series` (with a `query`) to find the `series_key`, then `get_macro_series`. Report the units and say when `is_stale` is true.

**Research questions.** For an analysed, cited answer rather than raw data, call `research_submit` (then `research_result` after `poll_after_s`). Use `depth: "deep"` (Deep Dive) only when the user asks for a thorough analysis: it takes longer and uses more of their Compute Units.

## Cost and limits
The user pays in Compute Units (CU): 0.5 CU per successful data call; `resolve_company`, `list_event_taxonomy`, `list_industries` and `list_macro_series` are free; research uses CU like a chat question. Each result shows `cu_charged`. Make targeted calls (a ticker and a date window) rather than broad sweeps. On `insufficient_balance` stop and tell the user. On `rate_limited` wait the stated seconds. Large results are trimmed with a note telling you which `limit` to page with.
