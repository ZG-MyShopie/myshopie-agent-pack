# Source sites: Amazon.de, Kaufland, OTTO

These are **sources** only. Destination listing channel is eBay via MyShopie.

## Amazon.de

- Prefer `/dp/ASIN` URLs.
- Extract: product title, “Info zu diesem Artikel” / About this item bullets, product information table (material, colour, brand, pieces), gallery images (`https://m.media-amazon.com/...`).
- Ignore: sponsored blocks, “frequently bought together”, seller chat, Amazon logistics claims.
- Rewrite bullets into original German paragraphs; keep factual pack sizes and materials.

## Kaufland

- Product detail pages under kaufland.de.
- Extract title, Beschreibung, Eigenschaften / attributes, price shown to staff, gallery images.
- Watch marketplace seller variance — confirm SKU/price with staff before creating the product.

## OTTO

- Product detail under otto.de.
- Extract title, Produktbeschreibung, material/care, image gallery.
- Same rewrite rules as Amazon (no verbatim long legal blocks).

## Rewrite rules

1. Write for the merchant’s store brand voice, not the source marketplace.
2. EBAY_DE → German primary; storefront may also keep EN.
3. Title ≤ 80 characters (eBay Trading limit).
4. Prefer structured channel HTML: headings + short paragraphs + bullet list for pack contents.
5. Include induction / hob compatibility only when the source states it.
6. Do not copy ASIN, Amazon standard product IDs, or “sold by Amazon” language into eBay copy.
