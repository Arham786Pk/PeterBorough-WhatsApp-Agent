# PeterBorough WhatsApp Agent

An n8n workflow that powers an AI customer-service agent for **Peterborough Plumbers** over WhatsApp. It handles inbound text and voice messages, qualifies plumbing leads through a structured booking flow, dispatches jobs to plumbers, and manages quote responses — all automatically.

## How It Works

```
Customer WhatsApp Message
  → Receive & normalise (text or audio)
  → Transcribe audio via OpenAI Whisper (if voice note)
  → De-duplicate & check bot-active flag
  → Load customer profile from backend
  → Master AI Agent (GPT-4 Turbo + conversation memory + knowledge search)
  → Capture reply, detect leads, save to backend
  → Send WhatsApp reply
  → Route plumber replies / quote responses back into the conversation
```

## Key Features

- **Voice note support** — downloads WhatsApp audio, transcribes with OpenAI Whisper, and feeds the transcript into the agent.
- **Structured booking flow** — enforces a 6-step booking process (name → postcode → address → date/time → summary → confirmation) before anything is marked as booked.
- **Returning-customer awareness** — loads existing profiles but always re-confirms details for new bookings.
- **Plumber dispatch loop** — routes accepted/rejected jobs, parses plumber replies, and sends rejection notices.
- **Quote handling** — detects quote responses from plumbers and relays them back to the customer.
- **Knowledge search** — the agent can query a knowledge base tool for pricing, service info, and FAQs.
- **Conversation memory** — buffer-window memory keeps context across messages in the same session.
- **Duplicate filtering** — prevents re-processing the same inbound message.

## Node Count

36 nodes covering: WhatsApp trigger, input normalisation, audio pipeline, AI agent with tools, lead persistence, plumber routing, and quote management.

## Prerequisites

| Dependency | Purpose |
|---|---|
| WhatsApp Business API credentials | Inbound trigger + outbound messaging |
| OpenAI API key | GPT-4 Turbo (agent + transcription) |
| Peterborough Plumbers backend API | Customer profiles, lead storage, message logging |

## Setup

1. Import `PeterBorough-Whatsapp-Agent.json` into your n8n instance.
2. Configure the **WhatsApp OAuth** credential (`WhatsApp OAuth-PeterBoroughPlumbers`).
3. Configure the **OpenAI API** credential.
4. Update backend URLs and API keys in the HTTP Request nodes to point to your environment.
5. Activate the workflow.

## Environment Variables / Secrets

The workflow references bearer tokens and API keys inline in HTTP Request nodes. Before going live, move these into n8n credentials or environment variables.

## License

Private / internal use.
