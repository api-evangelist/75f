---
name: Read 75F building data over the Haystack API
description: >-
  Authenticate against the 75F platform and pull live and historical building data — zone temperatures,
  CO2, setpoints, equipment state — out of a Project Haystack-tagged model, without writing anything.
api: https://api.75f.io/ph
generated: '2026-09-05'
method: generated
source: https://support.75f.io/hc/en-us/sections/5484889695123-API-s-for-Integrations
grounding: >-
  Every operation, header, parameter and error below is quoted from 75F's own published API reference.
  75F publishes no OpenAPI, so operations are named by their documented HTTP method and path rather than
  by operationId; nothing here is invented.
operations:
  - POST /oauth/token
  - GET /ph/read
  - GET /ph/hisReadMany
---

# Read 75F building data

## Before you start

You need three things, and none of them can be self-served:

1. A **Facilisight account** created with a standard username and password. If the account is federated
   to Microsoft 365 or Google it **cannot** be used from the API.
2. A **Secondary Manager** role on every site you intend to read. The customer's Facilisight
   administrator grants this.
3. An **approved product subscription** in the 75F developer portal
   (<https://api-management-75f-dev.developer.azure-api.net/>), which yields the subscription key. Keys
   can also be read in Facilisight under *Building Options → API Management → API Keys*.

Every call carries **two** credentials. Missing either one is a 401, and the two 401s look different —
see step 4.

## 1. Mint a bearer token

```
POST https://api.75f.io/oauth/token
Content-Type: application/x-www-form-urlencoded
Ocp-Apim-Subscription-Key: <your product subscription key>

grant_type=client_credentials&client_id=<facilisight-username>&client_secret=<facilisight-password>
```

A 200 returns `{"access_token": "...", "token_type": "bearer", "expires_in": "3600"}`. Cache it and
re-mint on expiry; do not mint one per request. Note the separate, longer clock 75F documents: the
client application credentials themselves expire within 24 hours and need reactivation.

## 2. Find the entities you care about

`read` takes a Haystack filter over tags, or a list of ids.

```
GET https://api.75f.io/ph/read?filter=point and siteRef==@<site-uuid> and zone and current and temp
Authorization: Bearer <token>
Ocp-Apim-Subscription-Key: <key>
Accept: application/json
```

Add `limit` to bound the result. There is no pagination — no cursor, no next link — so a truncated
result set cannot be resumed. Filter more narrowly instead.

The response is a Haystack grid: `metadata.ver` `"3.0"`, a `cols[]` array naming every tag present, and
`rows[]`. Ids come back as `"r:<uuid>"`; to feed one back into a request, write it as `@<uuid>`. Marker
tags serialize as `"m:"`.

If you do not know the ids, the operator can find them in the Facilisight **Site Explorer**, which
accepts the same filter syntax — build the query there first, confirm it returns what you expect, then
run it through the API.

## 3. Pull history

```
GET https://api.75f.io/ph/hisReadMany?ids=@<uuid>,@<uuid>&range=2026-09-01,2026-09-04
Authorization: Bearer <token>
Ocp-Apim-Subscription-Key: <key>
```

`range` accepts `today`, `yesterday`, `{date}`, `{date},{date}`, `{dateTime},{dateTime}` or a single
`{dateTime}` meaning everything after it. The range **includes the start and excludes the end**, and it
is evaluated in each point's own configured timezone — if you name a different zone, the server converts
before querying.

Grid metadata carries `id`, `hisStart` and `hisEnd`; rows are `ts`/value pairs.

**Batch your ids.** The ids go in the query string, and an over-long URL returns HTTP **414** with an
HTML body — not JSON, not ZINC. Split large id lists across several requests.

## 4. Read the failures correctly

| What you see | What it means |
|---|---|
| 401 with `{"statusCode":401,"message":"Access denied due to missing subscription key..."}` | No `Ocp-Apim-Subscription-Key` header |
| 401 with `{"statusCode":401,"message":"...invalid subscription key..."}` | Key is wrong, or the subscription is not active |
| 401 with **no body at all** | The bearer token is bad or expired — re-mint it |
| 404 with `{"statusCode":404,"message":"Resource not found"}` | Wrong gateway path |
| 404 with `ver:"3.0" err dis:"Requested entity not found" empty` | The filter matched nothing |
| 400 with `ver:"3.0" err dis:"Invalid filter: ..." empty` | Malformed Haystack filter |
| 414, HTML body | URL too long — batch the ids |

**The trap:** requesting points the account is not entitled to returns **HTTP 200 with an empty grid**,
not 403. An empty grid is not proof that there is no data — it may mean the Secondary Manager grant is
missing for that site. Check the grant before concluding the building has no sensors.

## Related

- Conventions and serialization: `conventions/75f-conventions.yml`
- Full error reference: `errors/75f-problem-types.yml`
- Entity graph and tags: `data-model/75f-data-model.yml`
