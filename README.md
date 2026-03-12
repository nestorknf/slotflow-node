# Slotflow Node.js SDK

Official Node.js SDK for the [Slotflow API](https://docs.slotflow.dev) — scheduling infrastructure for AI agents.

## Installation

```bash
npm install slotflow
```

## Usage

```typescript
import { SlotflowClient } from "slotflow";

const client = new SlotflowClient({
  token: "sk_live_your_api_key",
});

// List available slots
const slots = await client.slots.getAvailableSlots({
  humanId: "human-uuid",
  dateFrom: "2026-03-15",
  dateTo: "2026-03-20",
  duration: 30,
});

// Book a slot
const booking = await client.bookings.createBooking({
  human_id: "human-uuid",
  starts_at: "2026-03-15T14:00:00Z",
  duration: 30,
  attendee_name: "Alex Rivera",
  attendee_email: "alex@startup.io",
  metadata: {
    lead_id: "lead_123",
    source: "ai_sdr",
  },
});
```

## Resources

- [Documentation](https://docs.slotflow.dev)
- [API Reference](https://docs.slotflow.dev/api-reference)
- [Dashboard](https://app.slotflow.dev)
