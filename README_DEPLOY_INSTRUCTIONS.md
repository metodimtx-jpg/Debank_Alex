# DeBank Tracker (Render-ready)

This project monitors a DeBank wallet and sends Telegram notifications when:
- tokens are added/removed
- token balances change
- new transactions appear
- tracks total portfolio USD value
- baseline PnL tracking (create baseline on first run, or set with /setbaseline)

It also supports Telegram commands (via long polling):
- /portfolio — show current tokens (top 30)
- /value — show total portfolio value
- /changes — show last saved snapshot
- /setbaseline [value] — set baseline for PnL comparisons

## Deployment (Render)

1. Create a new GitHub repo and push the contents of this project.
2. Go to https://render.com -> New -> Blueprint -> Connect your GitHub repo.
3. Render will detect `render.yaml` and create a worker service.
4. Set environment variables in Render (Service -> Environment):
   - TELEGRAM_TOKEN = <your-telegram-bot-token>
   - CHAT_ID = <your numeric chat id> (e.g. 1827049213)
   - TARGET_ADDRESS = (optional) custom wallet address (defaults to the one requested)
   - CHECK_INTERVAL = (optional) seconds between checks (defaults to 30)
5. Deploy. The bot will start and send an initial message to your Telegram chat.

## Files
- app.py — main worker script (monitor + command handler)
- requirements.txt — pip deps
- render.yaml — Render blueprint config
- state.json — runtime state (auto-created by the bot)
- baseline.json — saved baseline for PnL (auto-created)

## Notes & Security
- Do NOT commit your TELEGRAM_TOKEN to GitHub. Use Render environment variables.
- The bot uses public DeBank endpoints. If any endpoint changes or is rate-limited, the bot will log errors.
- You can adjust CHECK_INTERVAL to lower frequency to avoid hitting public API limits.
- This project is provided as-is. Test it carefully before depending on it for critical alerts.
