---
name: myshopie-channel-listing-ops
description: >-
  Cross-channel listing ops via MyShopie MCP — read Amazon.de / Kaufland / OTTO
  source pages, rewrite copy, create a DRAFT MyShopie product, create an eBay DRAFT,
  and publish live only after human confirm:true. Use when staff ask to list from
  Amazon/Kaufland/OTTO onto eBay, import a competitor SKU, or batch channel listings.
---

# MyShopie channel listing ops (MCP)

## Prerequisites

1. Remote MCP at `https://mcp.myshopie.app/mcp` with Bearer `msh_…` key from **Settings → AI Agents**.
2. Key scopes: `products:write`, `marketplace:read`, `marketplace:write` (plus storefront media helpers if using image URL ingest).
3. Staff user behind the key must have permissions to create/edit products and manage apps.
4. Run `whoami` then `ebay_connection_status` first.
5. For classic-majority sellers, set `export_listing_model` to `classic` (do not rely on `auto`).

## Hard rules

- **Draft first — always.** Create MyShopie products as `draft`. Use only `create_ebay_listing_draft` for eBay (API create rejects `publish:true`). Classic drafts stay local until publish; do not treat “list on eBay” as live.
- Phrases like “list it” / “put on eBay” mean **draft** unless the human clearly says publish live / confirm publish.
- **Never** call `publish_ebay_listing` or `purge_unpublished_ebay_draft` without explicit human `confirm: true`.
- **Never** unpublish / EndItem / delete live eBay listings (platform forbids it).
- **Scan images + text for IP / marks — continue drafts, report at the end.** While importing, open/inspect **each** product picture and the listing text. Record a flag (do **not** stop the draft pipeline) if you see or read any of:
  - Trademark marks: ™, ®, or clear brand-logo lockups that look like third-party marks you should not redistribute
  - Copyright notices: ©, “Copyright”, “All rights reserved”, watermarked rights lines
  - CE (or similar conformity) marks/labels burned into the image or claimed in the copy when you cannot verify the merchant may use that claim
  - Source-marketplace watermarks / “Amazon’s Choice” style badges baked into the photo
  - If unsure whether a mark is present → **flag** (conservative)
  - Still create the MyShopie **draft** and eBay **draft** with the images; **never** publish live until the merchant has reviewed flags and then confirms publish
- **Rewrite** source copy — do not paste Amazon legal text verbatim; prefer German for `EBAY_DE`.
- Cap plain descriptions ~3500 chars (eBay inventory hard cap 4000).
- Never invent prices or stock — use values the merchant supplies.

## Happy path (one SKU)

1. Fetch source URL (Amazon.de / Kaufland / OTTO). Extract title, bullets, specs, image HTTPS URLs, price hint.
2. **IP / marks scan:** inspect every candidate image and the extracted text for ™ ® © / Copyright / CE / watermarks. Note hits (image URL + what was seen); **continue** drafting.
3. Rewrite DE title (≤80 chars) + paragraphs; optional EN; build channel HTML (still no verbatim Amazon legal text; avoid inventing CE/TM claims in copy).
4. `create_product_from_channel_brief` with `status: draft`, SKU, price, stock, `paragraphs_de`, `image_urls`.
5. Map MyShopie category → eBay leaf: `search_ebay_categories` → save mapping / product override via admin or category-map APIs.
6. `get_ebay_category_aspects` + `preflight_ebay_export` — fill **every** `required` aspect (product attributes / attribute-map). Example: `Geeigneter Herdtyp`.
7. `create_ebay_listing_draft` with optional `channel_description_html`.
8. Show draft summary (listing id, publish_state, missing aspects) **plus** any IP/marks flags, and the **MyShopie admin draft URL** (`…/products/{id}/edit` for that store) so the merchant can review visually. Ask what to do with flagged items (keep / replace image / remove image / hold publish).
9. Only after they guide you **and** explicitly approve: `publish_ebay_listing` with `confirm: true`.
10. Verify with `list_marketplace_listings` / remote by-sku / item URL.

## Batch mode + IP flags

- Create all MyShopie + eBay **drafts** without stopping on flags.
- At the **end**, return a table of flagged SKUs/listings: issue found, which image(s)/text, MyShopie draft admin URL, eBay draft id.
- Ask the merchant how to proceed per flagged row before any live publish.

## Inventory draft blocking a classic re-export

If an unpublished Inventory offer blocks the SKU:

1. Confirm remote offer is `UNPUBLISHED` with empty listing id.
2. `purge_unpublished_ebay_draft` with `confirm: true`.
3. Ensure `export_listing_model=classic`.
4. Create classic draft → publish with confirm.

## References

- `reference/workflow.md` — detailed checklist
- `reference/source-sites.md` — Amazon / Kaufland / OTTO extraction + rewrite rules
- `examples.md` — prompts
- Docs: https://docs.myshopie.com/ai-agents/
