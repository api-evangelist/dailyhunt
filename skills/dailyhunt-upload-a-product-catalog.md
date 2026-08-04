---
name: Upload a product catalog to Dailyhunt
description: Create a vendor product catalog in the Dailyhunt system and batch-submit product create, update and delete rows, then poll the asynchronous batch for completion and per-row errors.
api: openapi/dailyhunt-shopping-catalog-openapi.yml
operations:
  - createCatalog
  - getCatalog
  - submitProductBatch
  - getProductBatchStatus
generated: '2026-08-04'
method: generated
source: openapi/dailyhunt-shopping-catalog-openapi.yml, https://developer.dailyhunt.in/ads/docs/shopping-catalog/
---

# Upload a product catalog to Dailyhunt

Use this when a commerce vendor needs its product feed onboarded into Dailyhunt for shopping and
advertising surfaces.

## Before you start

The vendor must already be registered with Dailyhunt and hold an **access token**. There is no
self-service registration and no sandbox — the guide documents only the live host
`https://money.dailyhunt.in`. Ads and catalog support is `ads-tech-team@verse.in`.

The access token goes in the **JSON request body** as `access_token`, not in a header and not as a
query parameter. This is unusual; do not "helpfully" move it to an Authorization header.

## Step 1 — create the catalog (`createCatalog`)

`POST /shopping-catalog/v1/catalog/`

```json
{ "access_token": "<vendor access token>", "catalog_name": "GB" }
```

Returns `201 Created` with `{"catalog_id": "<uuid>"}`. **Store the `catalog_id`** — every later call
is scoped to it. Do this once per catalog, not once per upload.

## Step 2 — confirm it (`getCatalog`)

`GET /shopping-catalog/v1/catalog/{catalog_id}`

Returns `{"id", "name", "created_on"}`, where `created_on` is a unix timestamp.

## Step 3 — submit a product batch (`submitProductBatch`)

`POST /shopping-catalog/v1/catalog/{catalog_id}/batch`

```json
{
  "access_token": "<vendor access token>",
  "item_type": "PRODUCT_ITEM",
  "allow_upsert": false,
  "requests": [
    { "method": "CREATE", "retailer_id": 1234, "data": { } },
    { "method": "UPDATE", "retailer_id": 1234, "data": { } },
    { "method": "DELETE", "retailer_id": 1234 }
  ]
}
```

Rules that matter:

- `method` is one of `CREATE`, `UPDATE`, `DELETE`. `retailer_id` is **your own** product identifier
  and is the key Dailyhunt matches on — it must be stable across uploads or you will create
  duplicates instead of updating.
- `DELETE` rows carry no `data` object.
- `UPDATE` rows are partial: send only the fields that changed. The published example updates
  `age_group`, `category`, `color`, `size`, `shipping` and one custom label and leaves the rest
  alone. Note that sending `"shipping": []` or `"applinks": {}` clears those fields rather than
  leaving them untouched.
- `allow_upsert` controls whether a `CREATE` for an existing `retailer_id` is accepted as an update.
  Set it deliberately. It is not an idempotency key and Dailyhunt publishes no idempotency contract
  for this endpoint.

Product `data` fields documented in the guide: `name`, `description`, `brand`, `category`,
`product_type`, `price`, `sale_price`, `sale_price_start_date`, `sale_price_end_date`, `currency`,
`availability` (for example `in stock`), `condition` (for example `new`), `inventory`, `gtin`,
`color`, `size`, `pattern`, `gender`, `age_group` (for example `infant`, `newborn`), `url`,
`image_url`, `additional_image_urls[]`, `retailer_product_group_id` (variant grouping),
`shipping[]` (`country`, `region`, `service`, `price_value`, `price_currency`), `applinks`
(`android[]` with url/app_name/package/class, `iphone[]` with url/app_store_id/app_name), and
`custom_label_2` / `custom_label_3` / `custom_label_4`.

The response is `201 Created` with `{"handles": "<uuid>"}`. **This is a receipt, not a result.**
Nothing has been validated yet.

## Step 4 — poll the batch (`getProductBatchStatus`)

`GET /shopping-catalog/v1/catalog/{catalog_id}/batch/status?handler={handle}`

The guide sends the access token in the body of this GET:

```json
{ "access_token": "<vendor access token>" }
```

Response:

```json
{
  "data": [{ "status": "Finished", "errors_total_count": 0, "errors": [] }],
  "updated_on": 1234
}
```

Poll with backoff until `status` reaches `Finished`, then check `errors_total_count`. **Row-level
failures only ever appear here** — a `201` on the submission tells you nothing about whether any
individual product was accepted. A pipeline that fires and forgets will silently drop products.

## What is not published

- No failure status codes are documented for any of these four endpoints — only the 200/201 success
  shapes. Handle non-2xx defensively.
- No sandbox, no test vendor credentials, no test catalog.
- No batch size limit, no rate limit and no quota is published.
- No webhook or callback on batch completion — polling is the only mechanism.
- No enum is published for `status` beyond the `Finished` example, and no schema is published for the
  entries in `errors[]`.
