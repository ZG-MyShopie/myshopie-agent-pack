# MCP tool → Admin API map

MCP tools call the same Admin API paths your store uses, authenticated with the Bearer `msh_…` key from **Settings → AI Agents**.

## Meta

| Tool | Method | Path |
|---|---|---|
| whoami | — | session payload |
| list_abilities | — | scope list |

## Products

| Tool | Method | Path |
|---|---|---|
| list_products | GET | `/api/v1/products` |
| get_product | GET | `/api/v1/products/{id}` |
| create_product | POST | `/api/v1/products` |
| update_product | PUT | `/api/v1/products/{id}` |
| set_product_status | PATCH | `/api/v1/products/{id}/status` |
| delete_product | DELETE | `/api/v1/products/{id}` |

## Orders

| Tool | Method | Path |
|---|---|---|
| list_orders | GET | `/api/v1/orders` |
| list_orders_since | GET | `/api/v1/orders?updated_since=` |
| get_order | GET | `/api/v1/orders/{id}` |
| update_order_status | PATCH | `/api/v1/orders/{id}` |
| fulfill_order | POST | `/api/v1/orders/{id}/fulfillment` |
| refund_order | POST | `/api/v1/orders/{id}/refund` |

## Settings

| Tool | Method | Path |
|---|---|---|
| get_store_config | GET | `{storefront}/store/config` |
| get_general_settings | GET | `/api/v1/settings/general` |
| update_general_settings | POST | `/api/v1/settings/general` |

## Storefront designer

| Tool | Method | Path |
|---|---|---|
| list_themes / get_appearance | GET | `/api/v1/settings/appearance` |
| set_theme | PUT | `/api/v1/settings/appearance` |
| get_storefront_theme_schema | GET | `/api/v1/settings/storefront/theme-schema` |
| get_storefront_layout | GET | `/api/v1/settings/storefront/layout` |
| replace_storefront_layout | PUT | `/api/v1/settings/storefront/layout` |
| get/update theme settings | GET/PUT | `/api/v1/settings/storefront/theme-settings` |
| get/update header | GET/PUT | `/api/v1/settings/storefront/layout/header` |
| get/update footer | GET/PUT | `/api/v1/settings/storefront/layout/footer` |
| homepage sections | GET/PUT | `/api/v1/settings/storefront/layout/homepage` |
| apply_storefront_blueprint | multi | appearance + layout + theme-settings + preview-token |
| templates | GET/PUT/POST | `/api/v1/settings/storefront/templates/{type}` |
| create_storefront_preview_token | POST | `/api/v1/settings/storefront/preview-token` |
| publish_storefront | POST | `/api/v1/settings/storefront/layout/publish` |
| reset_storefront_layout | POST | `/api/v1/settings/storefront/layout/reset` |
| versions | GET/POST | `/api/v1/settings/storefront/versions…` |

## Content

| Tool | Method | Path |
|---|---|---|
| list_collections | GET | `/api/v1/collections/manage` |
| banners | CRUD | `/api/v1/banners` |
| announcements | CRUD | `/api/v1/announcements` |
| faqs | CRUD | `/api/v1/faqs` |
| social links | CRUD | `/api/v1/social-links` |

## CMS

| Tool | Method | Path |
|---|---|---|
| cms pages | CRUD | `/api/v1/cms-pages` |
| page layouts | GET/PUT/POST | `/api/v1/settings/storefront/pages/{id}/layout` |

## Media

| Tool | Method | Path |
|---|---|---|
| create_media_upload_url | POST | `/api/v1/media/upload-url` |
| confirm_media_upload | POST | `/api/v1/media/confirm` |
| upload_storefront_image | multi | upload-url → PUT object → confirm |

## Marketplace / eBay (channel listing ops)

| Tool | Method | Path |
|---|---|---|
| ebay_connection_status | GET | `/api/v1/apps/installed` (filtered) |
| search_ebay_categories | GET | `/api/v1/marketplace/category-search/ebay` |
| get_ebay_category_aspects | GET | `/api/v1/marketplace/category-aspects/ebay` |
| list_marketplace_listings | GET | `/api/v1/marketplace/listings` |
| get_marketplace_listing | GET | `/api/v1/marketplace/listings/product/{id}` |
| preflight_ebay_export | POST | `/api/v1/marketplace/preflight/export` + `/preflight/ebay-aspects` |
| create_product_from_channel_brief | POST | `/api/v1/products` (+ `/products/{id}/images-from-urls`) |
| create_ebay_listing_draft | POST | `/api/v1/marketplace/listings` (`publish:false`) |
| set_listing_channel_description | PATCH | `/api/v1/marketplace/listings/{id}/channel-description` |
| publish_ebay_listing | POST | `/api/v1/marketplace/listings/{id}/publish` (`confirm:true`) |
| purge_unpublished_ebay_draft | POST | `/api/v1/marketplace/listings/{id}/purge-unpublished-draft` |
| push_ebay_listing | POST | `/api/v1/marketplace/listings/{id}/push` |

See skill `myshopie-channel-listing-ops` and https://docs.myshopie.com/ai-agents/.
