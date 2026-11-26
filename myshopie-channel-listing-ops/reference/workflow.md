# Channel → MyShopie → eBay workflow checklist

## 0. Connection

- [ ] `whoami` — correct tenant, scopes include products + marketplace
- [ ] `ebay_connection_status` — installed, policies present, `export_listing_model` known
- [ ] Classic sellers: `export_listing_model=classic` (do not rely on `auto` if `merchant_location_key` is set)

## 1. Source extract

- [ ] Open Amazon.de / Kaufland / OTTO product URL
- [ ] Capture: title, bullet benefits, pack contents, materials, compatibility, image URLs (https), suggested price/qty from merchant
- [ ] If fetch blocked, ask staff to paste “About this item” / HTML

## 1b. IP / marks scan (continue drafting)

- [ ] Open **each** product image and inspect for ™ ® ©, Copyright / “All rights reserved”, CE (or similar) marks, brand watermarks, marketplace badges burned into the photo
- [ ] Scan title + description text for the same (™ ® © / Copyright / CE claims)
- [ ] If any hit **or** unsure → **record a flag** (image URL + what was seen); do **not** stop — keep creating MyShopie + eBay drafts
- [ ] Carry flags through to the end-of-run report

## 2. Rewrite

- [ ] German title ≤ 80 characters for EBAY_DE
- [ ] 4–6 original paragraphs (antihaft, Pflege, Lieferumfang, Herdarten, Schadstoffe/Geschenk)
- [ ] Optional EN translation for storefront
- [ ] Channel HTML (`h2`/`h3`/`p`/`ul`) stored via `channel_description_html`
- [ ] No Amazon trademarks / “Amazon’s Choice” / seller-specific claims
- [ ] Do not invent CE / TM / copyright claims in rewritten copy

## 3. MyShopie product

- [ ] `create_product_from_channel_brief` (`status: draft`)
- [ ] Images attached (`image_urls` or `POST /products/{id}/images-from-urls`) — include even if flagged; note flags separately
- [ ] Wait until media `status=completed` with https `cdn_url` before classic publish
- [ ] SKU unique — on 422 reuse existing product
- [ ] Build / keep MyShopie admin draft URL: `…/products/{id}/edit`

## 4. Category + aspects

- [ ] `search_ebay_categories` → map leaf category
- [ ] `preflight_ebay_export` — category ready
- [ ] `get_ebay_category_aspects` — list `required: true`
- [ ] Set product attributes / attribute-map so every required aspect has a value
- [ ] Re-run aspects preflight until `ready: true`

## 5. eBay draft

- [ ] `create_ebay_listing_draft` (`publish` false)
- [ ] Optional `set_listing_channel_description`
- [ ] Confirm local row: `publish_state=draft`, `listing_model` classic or inventory

## 6. Human gate

- [ ] Present title, price, stock, category, aspects, description excerpt, images count
- [ ] Present **MyShopie admin draft URL** for visual review
- [ ] If any IP/marks flags (this SKU or batch): list them clearly and ask what to do (keep / replace image / remove image / hold)
- [ ] Ask for explicit publish approval only after flags are addressed or accepted

## 7. Publish

- [ ] `publish_ebay_listing` with `confirm: true`
- [ ] Verify `publish_state=published`, `external_listing_id`, item URL
- [ ] On Trading aspect errors: fix product attributes, `push_ebay_listing` or republish draft path

## 8. Cleanup (unpublished Inventory only)

- [ ] `purge_unpublished_ebay_draft` with `confirm: true` only when never live
