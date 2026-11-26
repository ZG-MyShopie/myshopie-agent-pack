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

## IP / marks scan (images + text)

Before uploading any image to MyShopie or creating an eBay draft:

1. Visually inspect **every** gallery image (open the URL; do not skip).
2. Flag for **explicit merchant review** if the image or listing text shows:
   - ™ or ® (or obvious third-party trademark lockups)
   - © / “Copyright” / “All rights reserved” / rights watermarks
   - CE (or similar conformity) marks or claims you cannot verify the merchant may use
   - Source-marketplace watermarks or badges baked into the photo
3. If unsure → flag. Do not upload or push the flagged asset until the merchant chooses skip, replace, or approve.
4. Only merchant-cleared images go into `image_urls` / MyShopie / eBay.

## Rewrite rules

1. Write for the merchant’s store brand voice, not the source marketplace.
2. EBAY_DE → German primary; storefront may also keep EN.
3. Title ≤ 80 characters (eBay Trading limit).
4. Prefer structured channel HTML: headings + short paragraphs + bullet list for pack contents.
5. Include induction / hob compatibility only when the source states it.
6. Do not copy ASIN, Amazon standard product IDs, or “sold by Amazon” language into eBay copy.
7. Do not carry unapproved CE / trademark / copyright claims from the source into rewritten copy.
