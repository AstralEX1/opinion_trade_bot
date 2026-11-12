<div align="center">
  <a href="https://x.com/BidAsk_bot/status/2021481426999538115"><img src="https://img.shields.io/badge/%F0%9F%8F%86%20Winner-Opinion%20Builders%20Program%20%C2%B7%20Phase%202%20grant-8D6B13?style=for-the-badge&labelColor=171923" alt="Winner of the Opinion Builders Program, Phase 2 grant" /></a>
  <h1>BidAsk Bot</h1>
</div>

BidAsk Bot is a Telegram interface for trading prediction markets on [Opinion](https://app.opinion.trade). The hosted bot is documented with floating limit orders that follow market prices, plus order and price notifications.

🏆 **Winner of the Opinion Builders Program, Phase 2** (grant): [announcement on X](https://x.com/BidAsk_bot/status/2021481426999538115)

## Links

- [Open the Telegram bot](https://t.me/Bid_AskBot)
- [User documentation](https://opinionbot.gitbook.io/docs)
- [X updates](https://x.com/BidAsk_bot)
- [Opinion markets](https://app.opinion.trade)

## What is in this repository

- `bot/main.py` — an aiogram Telegram bot with wallet registration and a `/make_market` flow. It reads a market and its order book, collects the amount, YES/NO side and price offset, then asks for confirmation before placing a limit order.
- `simple_flow.py` — an interactive command-line flow for checking market details, spread and liquidity, choosing a side and price offset, and placing a limit order.

### Repository code and hosted bot

The public bot documentation describes additional hosted features, including floating orders and notifications. The source in this repository currently exposes `/make_market` and a one-time limit-order workflow; it may not match every command or feature available in the hosted bot. Use the [documentation](https://opinionbot.gitbook.io/docs) for the hosted bot's current instructions.

## Run locally

Install the dependencies and create a local environment file:

```bash
git clone https://github.com/AstralEX1/opinion_trade_bot.git
cd opinion_trade_bot
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
cp .env.example .env
```

Edit `.env` locally. Do not commit it.

| Variable | Used by | Purpose |
| --- | --- | --- |
| `BOT_TOKEN` | Telegram bot | Telegram bot token. |
| `MASTER_KEY` | Telegram bot | A random 32-byte key encoded as 64 hexadecimal characters. It encrypts and decrypts saved user data. |
| `RPC_URL` | Both | BNB Chain RPC endpoint (the code uses chain ID 56). |
| `ADMIN_TELEGRAM_ID` | Telegram bot, optional | Telegram user ID for the admin database-export command. |
| `API_KEY` | `simple_flow.py` | Opinion API key. |
| `PRIVATE_KEY` | `simple_flow.py` | Wallet private key for the standalone order flow. |
| `MULTI_SIG_ADDRESS` | `simple_flow.py` | Wallet or multisig address used by the SDK. |

`simple_flow.py` requires `API_KEY`, `RPC_URL`, `PRIVATE_KEY` and `MULTI_SIG_ADDRESS`. Add any missing values to your local `.env`; keep them out of source control.

Start the Telegram bot from the repository root:

```bash
python bot/main.py
```

Run the standalone order flow instead:

```bash
python simple_flow.py
```

## Security and trading risks

The Telegram implementation asks users for a wallet address, private key and Opinion API key. It encrypts those values with AES-GCM and stores them in `bot/users.db`; the running process uses `MASTER_KEY` to decrypt them when needed. Anyone operating a hosted instance controls that runtime and must protect both the key and database. Review the code and trust the operator before sending wallet credentials to a bot.

Use a wallet dedicated to this bot, keep only the funds needed for trading, and verify each market and order before confirming. Prediction-market trading can lose funds; this project is not financial advice and does not guarantee returns.