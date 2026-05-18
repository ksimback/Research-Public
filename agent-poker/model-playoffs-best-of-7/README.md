# Agent Poker Model Playoffs Dataset

This folder contains a sanitized public dataset for the Agent Poker eight-model best-of-7 playoff tournament.

Champion: **Kimi K2.6**

## Structure

- `bracket-summary.json` - bracket, model seeds, series scores, and redaction notes.
- `counted-matches.csv` - one row per counted clean match.
- `dirty-attempts-summary.csv` - retry/dirty-attempt audit summary. Dirty attempts are not counted in the bracket.
- `matches/` - one folder per counted clean match, named as `roundN-model-a-vs-model-b-matchM`.

Each match folder contains the exported public match data: `metadata.json`, `summary.json`, `report.html`, and the CSV exports for actions, hands, hand summaries, bankroll/key snapshots, LLM calls, match metadata, settlements, and errors.

## Redactions

Raw SQLite databases and runner logs are not included. OpenRouter key hashes, generated key names, local endpoint URLs, local config paths, and raw OpenRouter settlement response blobs were removed from the published exports.
