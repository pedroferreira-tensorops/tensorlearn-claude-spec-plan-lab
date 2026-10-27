# Specification

## Requirements

1. R-1 (I-1): Notify a patient only after a doctor has reviewed the result and it has been released; an upload alone never triggers a notification.
2. R-2 (I-2): Messages say only that an update is available in the authenticated portal. No clinical content in messages or logs; logs keep event IDs and delivery states only.
3. R-3 (I-3): Link to the existing portal route in src/portal/result-route.mjs; no portal redesign.
4. R-4 (I-4): Use the released result version as the idempotency key; bounded retries never create duplicate deliveries.
5. R-5 (I-4): Respect the provider limits of 50 requests/second and 20 messages per batch.

## Design

Enqueue notification jobs through src/service/queue.mjs once a result is reviewed and released. The job carries the released result version as its idempotency key and no clinical payload.

## Open decisions and owners

- OPEN-1: channel choice stays open; owners are product and privacy.
- OPEN-2: referring-doctor notifications are not in scope; owners are clinical and privacy.

## Traceability

| Intent | Requirement | Test |
| --- | --- | --- |
| I-1 | R-1 | T-1 release gate |
| I-2 | R-2 | T-2 privacy of message and logs |
| I-3 | R-3 | T-3 portal route reuse |
| I-4 | R-4, R-5 | T-4 duplicate retry, T-5 throughput boundary |
