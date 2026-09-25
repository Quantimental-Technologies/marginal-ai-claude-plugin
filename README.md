# Marginal AI plugin for Claude Code

Adds the Marginal AI MCP server (`https://api.marginal-ai.com/mcp`) and a `marginal-research` skill.

## Install
```
/plugin marketplace add Quantimental-Technologies/marginal-ai-claude-plugin
/plugin install marginal-ai@marginal-ai
```
Then run `/mcp`, choose `marginal-ai`, and sign in to Marginal AI in the browser window that opens. You need a Marginal AI account; the Agent API is included in every plan.

## What you get
- Corporate and market events (SEC 8-K items and corroborated news) with source links and point-in-time first-seen timestamps.
- Figures exactly as companies reported them to the SEC, each linked to its filing.
- Macroeconomic series.

Uses your Compute Units: 0.5 CU per data call (lookups are free); research questions cost like a chat question. Factual data only: Marginal AI does not give investment advice and does not place orders. Docs and limits: https://marginal-ai.com/agent-api. Terms: https://marginal-ai.com/agent-api-terms.
