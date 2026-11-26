# Example prompts

## Single SKU from Amazon

```
List this Amazon product onto eBay for our store via MyShopie MCP:
https://www.amazon.de/dp/XXXXXXXXXX
SKU DEMO-1001, price €49.90, stock 5.
Rewrite the German description (don't paste Amazon text).
Create a MyShopie draft + eBay draft only — do not publish until I confirm.
```

## Missing required aspect

```
preflight_ebay_export failed: missing Geeigneter Herdtyp.
Add Elektrisch, Gas, Induktion from the product facts, then recreate/push the draft.
Still do not publish without my confirm:true.
```

## Purge unpublished Inventory draft

```
SKU DEMO-1001 has an unpublished Inventory offer blocking classic export.
Purge only if it was never live, confirm:true, then create a classic eBay draft.
```

## Batch drafts

```
Here are 20 Amazon URLs + SKU/price/qty CSV.
Create MyShopie drafts + eBay drafts for each.
Stop before any publish; give me a table of listing ids to approve.
```
