# TickMineBot TON Connect Mini App

This is the starter production architecture for a Telegram Mini App using TON Connect.

## What is already included

- TickMineBot branding/logo
- Telegram Mini App bootstrap
- TON Connect wallet connect/disconnect
- TON Mainnet chain ID `-239`
- wallet address parsing with `@ton/core`
- wallet/reconnect UI
- withdrawal amount + destination validation
- review/approval screen
- backend API skeleton
- separation of off-chain mining balance from on-chain TICK
- no private key or seed phrase anywhere

TON Connect keeps wallet keys inside the user's wallet and lets the dApp request connection/transactions.

## 1. Frontend

```bash
cd frontend
npm install
cp .env.example .env
npm run dev
```

Before deployment, edit `public/tonconnect-manifest.json` and replace the placeholder domain with the real HTTPS frontend domain.

## 2. Backend

```bash
cd backend
npm install
cp .env.example .env
# fill TELEGRAM_BOT_TOKEN
# fill TICK_JETTON_MASTER after the real TICK Jetton is deployed
npm run dev
```

## 3. Telegram

Create/open your bot with BotFather and configure its Mini App URL to the deployed HTTPS frontend.

The Mini App URL must be public HTTPS.

## 4. TON Connect manifest

The manifest is served from:

`https://YOUR-DOMAIN/tonconnect-manifest.json`

Its `url` must exactly point to your deployed app and its `iconUrl` must be a public HTTPS image URL.

## 5. TICK

Do NOT put a fake master address into production.

The real TICK Jetton master contract address is the missing blockchain-specific value. Once it exists, implement the Jetton transfer message in `/api/withdraw/prepare`.

## 6. Production database

Recommended tables:

- users
- wallet_sessions
- withdrawal_requests
- transactions

Never store seed phrases or private keys.

## 7. Deploy

Frontend can be deployed to a static hosting service such as Vercel/Netlify/Cloudflare Pages.
Backend can be deployed to Render/Railway/Fly.io/etc.

Use separate environment variables for frontend/backend.

## 8. Security checklist

- Validate Telegram Mini App initData on the backend.
- Parse TON addresses; do not validate using `startsWith("UQ")`.
- Require Mainnet chain `-239` for production.
- Bind a withdrawal to the authenticated Telegram user and connected wallet.
- Make withdrawal IDs one-time/non-replayable.
- Validate amount and destination server-side.
- Do not trust client-side mining balance.
- Confirm the blockchain transaction before marking withdrawal `CONFIRMED`.
- Never request or store a private key/seed phrase.
