# Claude Code Statusline

A custom statusline for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that displays your current model, context window usage, and API usage limits at a glance.

```
Claude Opus 4.6 | Ctx: 12% | 5h: 34% | 7d: 8% | Dir: ~/Projects/my-app
```

## What it shows

- **Model** — the active Claude model
- **Context window** — percentage of context used in the current conversation
- **5-hour usage** — utilization against the rolling 5-hour rate limit
- **7-day usage** — utilization against the rolling 7-day rate limit
- **Directory** — current working directory

Usage percentages are color-coded: green (<50%), yellow (50–80%), red (>80%).

## Setup

1. Clone this repo:

   ```sh
   git clone https://github.com/tuckerritti/claude-statusline.git ~/Projects/claude-statusline
   ```

2. Symlink into your Claude config:

   ```sh
   ln -s ~/Projects/claude-statusline/statusline.py ~/.claude/statusline.py
   ```

3. Add the statusline to `~/.claude/settings.json`:

   ```json
   {
     "statusLine": {
       "type": "command",
       "command": "python3 ~/.claude/statusline.py"
     }
   }
   ```

## Requirements

- Python 3.10+
- Claude Code with OAuth credentials (automatically available when logged in)
- macOS or Linux

## How it works

Claude Code pipes JSON with session data (model, context window, cwd) to the statusline command via stdin. The script reads that data and also fetches your usage limits from the Anthropic API using your OAuth credentials.

API responses are cached for 2 minutes (`~/.claude/.usage_cache.json`) to avoid rate limits.
