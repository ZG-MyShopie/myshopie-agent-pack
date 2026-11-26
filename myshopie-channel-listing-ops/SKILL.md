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
- **Rewrite** source copy — do not paste Amazon legal text verbatim; prefer German for `EBAY_DE`.
- Cap plain descriptions ~3500 chars (eBay inventory hard cap 4000).
- Never invent prices or stock — use values the merchant supplies.

## Happy path (one SKU)

1. Fetch source URL (Amazon.de / Kaufland / OTTO). Extract title, bullets, specs, image HTTPS URLs, price hint.
2. Rewrite DE title (≤80 chars) + paragraphs; optional EN; build channel HTML.
3. `create_product_from_channel_brief` with `status: draft`, SKU, price, stock, `paragraphs_de`, `image_urls`.
4. Map MyShopie category → eBay leaf: `search_ebay_categories` → save mapping / product override via admin or category-map APIs.
5. `get_ebay_category_aspects` + `preflight_ebay_export` — fill **every** `required` aspect (product attributes / attribute-map). Example: `Geeigneter Herdtyp`.
6. `create_ebay_listing_draft` with optional `channel_description_html`.
7. Show draft summary (listing id, publish_state, missing aspects). Wait for human approval.
8. Only then `publish_ebay_listing` with `confirm: true`.
9. Verify with `list_marketplace_listings` / remote by-sku / item URL.

## Inventory draft blocking a classic re-export

If an unpublished Inventory offer blocks the SKU:

1. Confirm remote offer is `UNPUBLISHED` with empty listing id.
2. `purge_unpublished_ebay_draft` with `confirm: true`.
3. Ensure `export_listing_model=classic`.
4. Create classic draft → publish with confirm.

## Batch mode

- Create many MyShopie + eBay **drafts**.
- Staff review in admin `/dashboard` marketplace listings.
- Publish one-by-one or via admin bulk publish after review — still require confirm per MCP publish call.

## References

- `reference/workflow.md` — detailed checklist
- `reference/source-sites.md` — Amazon / Kaufland / OTTO extraction + rewrite rules
- `examples.md` — prompts
- Docs: https://docs.myshopie.com/ai-agents/
