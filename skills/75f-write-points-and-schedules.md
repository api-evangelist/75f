---
name: Change 75F building behaviour — point writes and special schedules
description: >-
  Override a writable point or create an exception-based schedule on a 75F-controlled building, with the
  authorization, priority-array and reversal rules that make the change safe to undo.
api: https://api.75f.io
generated: '2026-09-05'
method: generated
source: https://support.75f.io/hc/en-us/articles/6012128424851-PointWrite-API
grounding: >-
  Operations, headers, response shapes and error messages are quoted from 75F's published reference.
  75F publishes no OpenAPI, so operations are named by documented HTTP method and path. No time window
  is asserted anywhere below, because 75F publishes none.
operations:
  - POST /oauth/token
  - GET /ph/read
  - POST /ph/rw/pointWrite
  - POST /v2/schedules/special
  - PUT /v2/schedules/special/{scheduleId}
  - DELETE /v2/schedules/special/{scheduleId}
---

# Change 75F building behaviour

> **These calls move real HVAC equipment in an occupied building.** There is no dry-run parameter, no
> validate-only mode and no idempotency key anywhere in the 75F API. Rehearse against the `/staging` or
> `/qa` gateway prefix, or in the Facilisight API Trial console, before touching production.

## 0. Prerequisites

Same as the read skill — a standard-auth Facilisight account, **Secondary Manager** on the target site,
and a **write-capable** product subscription. Read and write keys are separate products in 75F's own
key taxonomy (Read API / Write API / Special Schedule API), so a read key will not write.

## 1. Find out what you are even allowed to move

Only points carrying the Haystack **`writable`** tag can be written. Relays, analog outputs and direct
physical components are not addressable at all.

```
GET https://api.75f.io/ph/read?filter=writable and siteRef==@<site-uuid>
```

75F is explicit that the obvious control point is often not the writable one. To move a VAV damper you
do **not** override `damperPosition` — it is not writable. You query the related config points
(`damper and config and (min or max)`) and override those. Expect a single physical outcome to require
several point writes, and confirm the mapping in Site Explorer before you act.

## 2. Write a point

```
POST https://api.75f.io/ph/rw/pointWrite
Authorization: Bearer <token>
Ocp-Apim-Subscription-Key: <write key>
Content-Type: application/json
```

The response is **not** an empty grid — 75F deviates from the Project Haystack spec here deliberately
and returns the resulting priority array:

```
ver:"3.0"
duration,level,val,who
0,8,0,"ccu_@236fea0f-d6ba-4372-a53d-9e16c8ba5d5d"
0,9,1,"api_BGS"
```

Read it before you assume the write took effect. `who` tells you who holds each level — a CCU writes as
`ccu_@<uuid>`, an API client as `api_<name>` — and a level numerically above yours wins. HTTP status
also signals success or failure here, another documented deviation from the Haystack spec.

**To undo:** write again at the same level, or release the level. There is no separate revert
operation, and 75F publishes no auto-release duration — a level you take, you keep until something
takes it back.

## 3. Create a special schedule

```
POST https://api.75f.io/v2/schedules/special
Authorization: Bearer <token>   # scope schedules:write
Ocp-Apim-Subscription-Key: <special schedule key>
Content-Type: application/json

{
  "zoneId": "zone-123",
  "date": "2026-11-26",
  "startTime": "08:00",
  "endTime": "18:00",
  "mode": "occupied",
  "setpoints": { "cooling": 74, "heating": 68 },
  "description": "Holiday override"
}
```

Success returns `{"status":"success","scheduleId":"sched-456"}`. **Keep the `scheduleId`** — it is the
only handle you have for the undo in step 4.

Validation the server enforces, so you do not have to guess:

- `date` must be `YYYY-MM-DD` and not in the past
- `startTime` must be before `endTime`
- `mode` is one of `occupied`, `unoccupied`, `custom`
- if `setpoints` are present, `heating` must be below `cooling`
- `zoneId` must be a zone the caller is authorized for
- the window must not overlap another special schedule on the same zone and date

Deadbands, setbacks and user limits are validated by the same engine as ordinary zone schedules, and
building-level constraints still apply unless explicitly overridden.

Failures: `400` invalid time range · `409` conflicting schedule already exists · `403` not authorized
for this zone · `422` missing required fields.

## 4. Reverse it

```
PUT    https://api.75f.io/v2/schedules/special/{scheduleId}   # amend
DELETE https://api.75f.io/v2/schedules/special/{scheduleId}   # remove
```

`DELETE` returns `{"status":"deleted"}`. **75F publishes no window for this** — no stated cut-off, and no
statement about whether a schedule can be deleted once it has started executing. Do not assume one, and
do not tell an operator there is one.

## What has no undo

`POST /ph/rw/hisWriteMany` writes timestamped values into the historian and 75F documents no delete or
correction operation for historized data. Writing a corrected value at the same timestamp is the only
recourse, and the docs do not state that it supersedes the earlier one. Treat history writes as
one-way.

## Related

- Reversibility, idempotency and dry-run posture: `conventions/75f-conventions.yml`
- Error reference: `errors/75f-problem-types.yml`
- Non-production gateways and consoles: `sandbox/75f-sandbox.yml`
