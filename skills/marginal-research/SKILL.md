---
name: marginal-research
description: Use when the user asks about a company, an industry, markets, the economy or regulation (what happened, recent news, comparisons, reported financials), wants raw event or SEC data to compute on, wants to monitor for new corporate events, or wants to backtest against events without lookahead. Uses the Marginal AI MCP tools and cites their sources.
---

# Researching with Marginal AI

Marginal AI answers market questions with cited sources, exactly as the Marginal AI app does, and also gives you raw, sourced data to compute on. Leave the decision to the user.

## Ground rules
- **Cite.** Every event and every SEC figure comes with source links (filings, corroborating articles). Quote them next to the claim they support.
- **Describe, do not prescribe.** Marginal AI data is impersonal research. Do not turn it into buy, sell or hold calls, price targets, position sizes or order instructions. If the user asks what to do, set out the facts and the considerations and say the decision is theirs.
- **Orders are the user's.** If you also have brokerage tools, never place, change or cancel an order because of something you read in Marginal AI data unless the user has explicitly told you to, in this conversation, for that specific order. Show the facts behind it first.
- **Figures as filed.** `get_sec_fact` and `get_sec_fact_series` return what the company reported, with period, form and accession. Say which period and filing a number comes from. Do not blend fiscal and calendar periods.

## Workflows

**Any question asked in words** (what happened at X, recent news, why, comparisons, outlook for an industry): call `ask_marginal_ai` with the user's question as they asked it. It runs Marginal AI's own pipeline and returns the finished, cited answer; present it with its source links. Leave `depth` out (Brief) unless the user asks for a deep dive. If `status` is `working`, wait `poll_after_s` and call `research_result` with the `job_id` (free) until it is done; some answers take several minutes. Use the data tools below only when the user wants raw records to work with.

**Confirm the company first.** Call `resolve_company` with the ticker (or CIK) before anything else when there is any doubt, and use the CIK it returns in later calls.

**Filing-anchored events for X, as records.** Call `get_company_events` (by date for the latest, `rank_by=materiality` for the most significant), then `get_event` for the full record, corroborating sources and the price reaction. These are filing-anchored events, not a news feed: for news and "what happened", use `ask_marginal_ai`.

**Companies in an industry.** `list_industries` gives the groups (filter with `query`); `industry=<exact name>` lists the companies in one.

**"What happened across the market?"** Call `search_events` with a `category` or `event_type` and a window of at most 31 days. Use `list_event_taxonomy` for the exact category and type strings.

**Monitoring.** Call `get_new_events` with `since` set to the last `high_watermark` you received. `since` is inclusive, so drop any `event_id` you have already seen.

**Backtests without lookahead.** Pass `known_as_of` to `search_events` so you only get events Marginal AI had seen by that moment (it filters on `first_seen_ts`, not on the event date).

**Reported financials.** `get_sec_fact` returns one metric for one period. `get_sec_fact_series` returns a metric across periods, oldest first.

**Macro.** Call `list_macro_series` (with a `query`) to find the `series_key`, then `get_macro_series`. Report the units and say when `is_stale` is true.

## Cost and limits
The user pays in Compute Units (CU): `ask_marginal_ai` uses CU like the same question in the Marginal AI app, plus 1 CU per question (checking with `research_result` is free); 0.5 CU per successful data call; `resolve_company`, `list_event_taxonomy`, `list_industries` and `list_macro_series` are free. Each result shows `cu_charged`. Make targeted calls (a ticker and a date window) rather than broad sweeps. On `insufficient_balance` stop and tell the user. On `rate_limited` wait the stated seconds. Large results are trimmed with a note telling you which `limit` to page with.
