---
name: myshopie-store-ops
description: Manage a MyShopie store via MCP — products, orders, general settings, and full storefront design (theme, header/footer, homepage, templates, CMS, content). Use when the merchant asks to update catalog, fulfill orders, change store contact info, or build/customize the online store by chat.
---

# MyShopie store operations (MCP)

## Prerequisites

1. Remote MCP URL + Bearer `msh_…` key from **Settings → AI Agents**.
2. Run `whoami` first to confirm store context and granted scopes.

## Workflow patterns

### Products

1. `list_products` with search/filters → identify ids.
2. `get_product` before edits when structure is unclear.
3. `create_product` / `update_product` / `set_product_status` for changes.
4. `delete_product` only with explicit merchant confirmation (`confirm: true`).

### Orders

1. `list_orders` or `list_orders_since` for monitoring.
2. `get_order` for details before status changes.
3. `update_order_status` or `fulfill_order` for fulfillment.
4. `refund_order` only after merchant confirms amount/reason (`confirm: true`).

### General settings

1. `get_general_settings` → read current values.
2. `update_general_settings` with a minimal patch (only fields being changed).

### Storefront designer (highest care)

Always **draft → preview → publish**. Prefer schema-guided tools; never invent section types.

#### Empty store / from-scratch landing page

1. `list_themes` → `set_theme` (use `confirm_unsupported: true` only when merchant accepts dropped sections).
2. `get_storefront_theme_schema` — learn allowed section types, shell fields, locales.
3. Seed content sources if needed:
   - Heroes: `upload_storefront_image` → `create_banner` (`type` hero)
   - FAQ accordion: `create_faq`
   - Announcement bar: `create_announcement`
   - Footer social: `create_social_link`
   - Featured collection: `list_collections`
4. Prefer **`apply_storefront_blueprint`** with:
   - `shell.header` / `shell.footer`
   - `sections` (homepage)
   - `settings_data` (colors/typography overlays)
5. Share `preview_url` from the blueprint result (or `create_storefront_preview_token` with optional `path`).
6. Visually QA (open preview in browser / computer-use). Iterate with `update_header`, `update_footer`, `update_homepage_section`, `update_storefront_theme_settings`.
7. Only after explicit merchant approval: `publish_storefront` with `confirm: true`.

#### Incremental homepage edits

1. `get_homepage_sections`
2. `update_homepage_section` / `add_homepage_section` / `reorder_homepage_sections` (supports `blocks` + `block_order`)
3. Preview → publish as above.

#### Templates & CMS pages

- Templates: `get_storefront_template` / `update_storefront_template` (`collection`, `product`, `contact`, `faq`, …)
- CMS: `create_cms_page` (+ `use_storefront_layout: true`) → `update_cms_page_layout` → preview with `path: /p/{slug}`

#### MCP prompts / resources

- Prompts: `design_homepage`, `redesign_header_footer`, `add_cms_page`
- Resources: `storefront://schema`, `storefront://layout/draft`, `storefront://preview`

Never call `reset_storefront_layout` unless the merchant explicitly asks to discard all customizations (`confirm: true`).

## Scope reference

| Scope | Tools |
|---|---|
| `products:read` | list/get products |
| `products:write` | create/update/delete/status |
| `marketplace:read` | eBay connection/listings/preflight (see `myshopie-channel-listing-ops`) |
| `marketplace:write` | eBay draft/publish/purge/push (confirm required for publish/purge) |
| `orders:read` | list/get orders |
| `orders:manage` | status, fulfill, refund |
| `settings:read/write` | general settings |
| `storefront:read/write` | theme, layout, content, CMS, media, preview, publish |

## Error handling

- **403**: Key scope or staff permission missing — tell the merchant which scope to enable when creating a new key.
- **422 validation**: Re-read `get_storefront_theme_schema` / `storefront://schema` and fix setting types/keys.
- **Theme requires_confirmation**: Retry `set_theme` with `confirm_unsupported: true` only after merchant OK.
- **Rate limits**: ~60 req/min per key — batch reads, avoid redundant `whoami` calls.

See `reference/api-map.md` and `examples.md` in this folder.
