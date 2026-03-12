# Slotflow Node.js SDK

[![npm version](https://img.shields.io/npm/v/slotflow.svg)](https://www.npmjs.com/package/slotflow)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

Official Node.js/TypeScript SDK for the [Slotflow API](https://slotflow.dev) — scheduling infrastructure for AI agents.

Slotflow lets your AI agent check availability, book appointments, and manage the full booking lifecycle for real humans — without breaking the agent workflow.

## Installation

```bash
npm install slotflow
```

## Quick Start

```typescript
import { SlotflowClient } from "slotflow";

const client = new SlotflowClient({
  token: "sk_live_your_api_key",
});

// 1. Create a human whose time can be booked
const human = await client.humans.createHuman({
  name: "Sarah Chen",
  email: "sarah@acme.com",
  role: "sales_rep",
  timezone: "America/New_York",
});

// 2. Find available slots
const { slots } = await client.slots.getAvailableSlots({
  humanId: human.data.id,
  date_from: "2026-03-15",
  date_to: "2026-03-20",
  duration: 30,
});

// 3. Book a slot
const booking = await client.bookings.createBooking({
  human_id: human.data.id,
  starts_at: slots[0].start,
  duration: 30,
  attendee_name: "Alex Rivera",
  attendee_email: "alex@startup.io",
  metadata: {
    lead_id: "lead_8294",
    conversation_id: "conv_1847",
    source: "ai_sdr",
  },
});
```

## Features

- **Full TypeScript support** — typed requests, responses, and errors
- **All API resources** — humans, availability, schedule overrides, slots, bookings, webhooks
- **Automatic retries** — built-in retry with configurable attempts (default: 2)
- **Timeouts** — configurable per-client and per-request (default: 60s)
- **Error handling** — typed error classes for 402, 404, 409, 422 responses
- **Custom fetch** — bring your own fetch implementation for any runtime

## API Resources

| Resource | Methods |
|---|---|
| `client.humans` | `createHuman`, `getHuman`, `listHumans`, `deleteHuman` |
| `client.availability` | `setAvailability` |
| `client.scheduleOverrides` | `createOverride`, `listOverrides`, `deleteOverride` |
| `client.slots` | `getAvailableSlots` |
| `client.bookings` | `createBooking`, `listBookings`, `cancelBooking` |
| `client.webhooks` | `createWebhook`, `listWebhooks`, `deleteWebhook` |

## Error Handling

```typescript
import { SlotflowClient, Slotflow } from "slotflow";

try {
  await client.bookings.createBooking({ /* ... */ });
} catch (error) {
  if (error instanceof Slotflow.ConflictError) {
    // 409 — slot was just booked by someone else, retry with next slot
  }
  if (error instanceof Slotflow.PaymentRequiredError) {
    // 402 — booking limit reached
  }
  if (error instanceof Slotflow.NotFoundError) {
    // 404 — human not found
  }
  if (error instanceof Slotflow.UnprocessableEntityError) {
    // 422 — validation error (outside working hours, invalid duration, etc.)
  }
}
```

## Configuration

```typescript
const client = new SlotflowClient({
  token: "sk_live_your_api_key",

  // Override the default timeout (seconds)
  timeoutInSeconds: 30,

  // Override the default retry count
  maxRetries: 3,

  // Custom fetch implementation (e.g. for edge runtimes)
  fetch: myCustomFetch,
});
```

Per-request overrides:

```typescript
await client.bookings.listBookings({}, {
  timeoutInSeconds: 10,
  maxRetries: 0,
});
```

## Requirements

- Node.js 18+
- Get your API key from the [Slotflow Dashboard](https://app.slotflow.dev)

## Links

- [Documentation](https://docs.slotflow.dev)
- [API Reference](https://docs.slotflow.dev/api-reference)
- [Dashboard](https://app.slotflow.dev)
- [Status](https://slotflow.dev)

## License

MIT
