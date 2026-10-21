# Annapurna Tiffin Service: website chatbot (Session 10)

A website with an AI chatbot for **Annapurna Tiffin Service**, a fictional home-style vegetarian tiffin service in Ludhiana. Built with the Session 10 starter kit, following the same steps as my first bot (Gill Sewing Machine Parts).

- **Live website:** https://annapurna-tiffin.jyotiverma96820.workers.dev
- **Bot backend:** https://annapurna-tiffin-bot.jyotiverma96820.workers.dev/chat (Cloudflare Worker)
- **Test score:** 15 / 15 (see [test-results.md](test-results.md))

*All business details are fictional and contact details are placeholders. This is a classroom demo.*

## How it works

```
Customer types in the chat  →  site/chat-widget.js  →  worker.js on Cloudflare  →  AI model (Groq, openai/gpt-oss-120b)
                                                              ↓
                                  When the customer confirms an order, it goes to the owner
                                  (Google Sheet and Telegram)
```

1. The customer chats on the website.
2. The worker adds the tiffin service's facts and rules, then asks the AI model for a reply.
3. When the customer confirms an order, the worker turns it into structured data with a reference such as `AT-261021-9366` and sends it to the owner's Google Sheet and Telegram.

## Files

| File | What it is |
|---|---|
| `claude.md` | The business card: every fact the bot is allowed to use. |
| `worker.js` | The bot backend. Section 2 holds the business facts and Section 3 the bot's rules, both written for Annapurna Tiffin Service. Sections 1 and 4 are unchanged from the starter kit. |
| `site/index.html` | One-page website (menu, prices, monthly plans, delivery areas, hours, contact) with its own CSS and system fonts only. It works on phones. |
| `site/chat-widget.js` | The chat bubble, copied unchanged from the starter kit. |
| `tests.md` | The 15 test questions and what a pass looks like. |
| `run-tests.js` | Sends the 15 tests to the live bot (Node.js, no extra packages). |
| `test-results.md` | Results table: question, reply, PASS or FAIL, and the reason. |
| `test-raw.json` | The bot's full replies from the test runs. |

## What the bot does

- Answers questions about the menu, prices, monthly plans, delivery areas, timings and Jain food.
- **Can quote prices** (Standard Rs 90, Deluxe Rs 130, plans from Rs 2,200), and always adds that festival-week prices are confirmed on WhatsApp.
- Helps customers choose a tiffin or plan by asking how many people, lunch or dinner, and their area.
- Takes orders: it shows a summary and asks "Shall I send this to our team?" before sending.
- **Priority:** high for monthly plans, office or party orders of 20 or more, and same-day orders; low for a single trial meal; normal for everything else.

## What it never does

- Give discounts or change prices.
- Promise delivery outside Sarabha Nagar, Model Town, BRS Nagar, PAU campus area and Ghumar Mandi.
- Promise that food is free from an allergen (the kitchen is shared).
- Confirm a same-day order as certain.
- Give medical or diet advice.
- Answer off-topic questions, or follow requests to ignore its rules.

## Testing

```bash
node run-tests.js        # all 15 tests, about 2 minutes
node run-tests.js 15     # one test only
```

Test 15 (a full order conversation) first marked a monthly plan as "normal" priority. One change to the priority rule in Section 3 fixed it, as described at the end of [test-results.md](test-results.md).

## Settings (in Cloudflare, not in this repo)

`LLM_BASE_URL`, `LLM_MODEL`, `LLM_API_KEY` (secret), `ALLOWED_ORIGINS`, plus `LEAD_WEBHOOK_URL`, `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` for sending orders to the owner. **No keys are stored in this repository.**

## Deploying the website

```bash
npx wrangler deploy --assets=./site --name annapurna-tiffin --compatibility-date 2026-10-12
```
