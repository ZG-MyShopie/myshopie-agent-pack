# Example merchant prompts

Use these with Claude Desktop, Cursor, or any MCP client connected to MyShopie.

## Products

- "List my 10 most recently updated products."
- "Set product #42 to draft status."
- "Create a simple product called Organic Honey — €12.99, one default variant, active."

## Orders

- "Show orders updated in the last 24 hours."
- "What's the status of order #1001? List line items."
- "Mark order #1001 as shipped and add tracking DHL1234567890." *(fulfill_order with appropriate payload)*

## Settings

- "What is our store contact email?"
- "Change the support email to hello@example.com."

## Storefront designer — from scratch

- "Use the Prism theme. Build a fashion landing page: announcement bar with Free shipping over €50, sticky header with Shop/About/Contact, hero with Summer Sale, featured collection, trust badges, FAQ, and a footer with newsletter + Instagram. Give me a preview link."
- "Our storefront is empty — design a clean home page for a coffee brand in German and English."
- "Switch theme to zenreh, keep my products, and redesign header/footer only."

## Storefront UI — incremental

- "What sections are on our homepage?"
- "Change the hero headline to Summer Sale — 30% off everything."
- "Make the primary button color #1a365d."
- "Add a featured products section below the hero."
- "Generate a preview link so I can review before we go live."
- "Open a preview of the contact page draft." *(create_storefront_preview_token path=/contact)*
- "Publish the storefront." *(only after merchant confirms — use confirm: true)*

## Content sources

- "Create three FAQ entries about shipping and returns."
- "Add a hero banner using this image URL: https://…"
- "Add Instagram and TikTok links to the footer."

## CMS

- "Create an About page at /p/about with a media-with-text section and team grid."
- "Add a Returns policy page under Legal and show it in the footer."

## Monitoring

- "Any new orders since yesterday at 9am?" *(list_orders_since with ISO8601 timestamp)*
