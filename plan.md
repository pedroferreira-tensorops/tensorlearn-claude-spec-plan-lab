# Implementation plan

## Ordered steps

1. Add the review-and-release gate before enqueueing (R-1).
2. Build the neutral message with the existing portal route (R-2, R-3).
3. Enqueue with the released result version as idempotency key and bounded retries (R-4).
4. Batch at most 20 messages and pace to 50 requests/second (R-5).

## Paths and dependencies

Step 2 depends on step 1; steps 3 and 4 use src/service/queue.mjs and depend on step 2. The route comes from src/portal/result-route.mjs.

## Risks and verification

Risks: duplicate deliveries on retry, clinical data leaking into logs, provider throttling. Tests: release gating, privacy of message and logs, duplicate retries, and throughput boundaries at 50/51 requests and 20/21 messages.

| Requirement | Step | Existing path | Test |
| --- | --- | --- | --- |
| R-1 | 1 | src/service/queue.mjs | T-1 |
| R-2 | 2 | src/portal/result-route.mjs | T-2 |
| R-3 | 2 | src/portal/result-route.mjs | T-3 |
| R-4 | 3 | src/service/queue.mjs | T-4 |
| R-5 | 4 | src/service/queue.mjs | T-5 |
