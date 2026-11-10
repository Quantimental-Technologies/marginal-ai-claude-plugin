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

Uses your Compute Units: a question costs like the same question in the Marginal AI app, plus 1 CU; a data call 0.5 CU (lookups are free). Factual data only: Marginal AI does not give investment advice and does not place orders. Docs and limits: https://marginal-ai.com/agent-api. Terms: https://marginal-ai.com/agent-api-terms.
