# MyShopie Agent Pack

Skills and safety rules so AI clients (Cursor, Claude, Codex, and other MCP clients) can manage your MyShopie store via the hosted MCP server.

**MCP (runtime):** `https://mcp.myshopie.app/mcp`  
**Docs:** https://docs.myshopie.com/ai-agents/  
**Releases:** https://github.com/ZG-MyShopie/myshopie-agent-pack/releases

---

## What you get

| Path | Use when |
| --- | --- |
| [`myshopie-store-ops/`](myshopie-store-ops/) | Products, orders, general settings, storefront designer |
| [`myshopie-channel-listing-ops/`](myshopie-channel-listing-ops/) | Amazon / Kaufland / OTTO → MyShopie → eBay draft listings |
| [`rules/`](rules/) | Cursor rules: confirm destructive ops; respect 403s |
| [`CLAUDE_CHANNEL_LISTING_HANDOFF.md`](CLAUDE_CHANNEL_LISTING_HANDOFF.md) | One-file paste for Claude (channel listing) |

---

## 1. Create an API key

1. Open your store admin → **Settings → AI Agents**.
2. **Create API key** and enable the scopes you need:

| Scope | Needed for |
| --- | --- |
| `products:read` / `products:write` | Catalog |
| `orders:read` / `orders:manage` | Orders, fulfill, refund |
| `settings:read` / `settings:write` | General settings |
| `storefront:read` / `storefront:write` | Theme, layout, CMS, publish |
| `marketplace:read` / `marketplace:write` | Channel listing / eBay drafts |

3. Copy the `msh_…` key once — it is shown only at creation time.

---

## 2. Connect MCP

**URL:** `https://mcp.myshopie.app/mcp`  
**Auth:** `Authorization: Bearer msh_…your_key…`

Health check: `https://mcp.myshopie.app/health`

After connecting, ask the agent to run **`whoami`** so you confirm store context and granted scopes.

### Config snippet (generic)

```yaml
url: https://mcp.myshopie.app/mcp
headers:
  Authorization: Bearer msh_…your_key…
```

---

## 3. Install skills by client

### Cursor

1. Clone or download this repo (or a [release](https://github.com/ZG-MyShopie/myshopie-agent-pack/releases)).
2. Copy skill folders into your project (or globally):

```bash
cp -R myshopie-store-ops ~/.cursor/skills/
cp -R myshopie-channel-listing-ops ~/.cursor/skills/   # if you need listing ops
cp rules/*.mdc ~/.cursor/rules/
```

3. Add the remote MCP server in Cursor MCP settings with the URL + Bearer header above.
4. Start a chat; the skills describe workflows the model should follow when calling MCP tools.

### Claude Desktop / Claude Code

1. Add a remote MCP server in Claude settings:
   - URL: `https://mcp.myshopie.app/mcp`
   - Header: `Authorization: Bearer msh_…`
2. Paste the relevant skill into project instructions or a custom skill:
   - Store ops: open [`myshopie-store-ops/SKILL.md`](myshopie-store-ops/SKILL.md)
   - Channel listing: prefer [`CLAUDE_CHANNEL_LISTING_HANDOFF.md`](CLAUDE_CHANNEL_LISTING_HANDOFF.md) (complete handoff) or [`myshopie-channel-listing-ops/SKILL.md`](myshopie-channel-listing-ops/SKILL.md)

### Codex / other MCP clients

1. Register the same remote MCP URL + Bearer token in the client’s MCP config.
2. Attach or paste `myshopie-store-ops/SKILL.md` (and channel-listing skill if needed) as agent instructions.
3. Call `whoami`, then use the tools listed in each skill.

---

## 4. Safety defaults

- **Storefront:** draft → preview → `publish_storefront` only after you confirm (`confirm: true`).
- **eBay:** `create_ebay_listing_draft` first; `publish_ebay_listing` only with `confirm: true`.
- **`confirm: true` required for:** delete product, refund, publish storefront, publish eBay listing, purge unpublished eBay draft, reset storefront layout, remove homepage section, delete banner/FAQ/CMS/social, restore-with-publish, blueprint `reset_first`.
- On **403**, check whether the **API key scope** or the **staff permission** is missing — do not retry blindly.
- Rate limit: about **60 requests/min** per key — batch reads; avoid redundant `whoami` calls.

---

## Example prompts

See [`myshopie-store-ops/examples.md`](myshopie-store-ops/examples.md) and [`myshopie-channel-listing-ops/examples.md`](myshopie-channel-listing-ops/examples.md).

Quick checks:

- “Run whoami and tell me my store and scopes.”
- “List my 10 most recently updated products.”
- “Show orders updated in the last 24 hours.”

---

## Versioning

| Change | Pack version |
| --- | --- |
| Breaking MCP tool renames / removed tools | **major** |
| New tools or new skill sections | **minor** |
| Typos, examples, docs only | **patch** |

Tag format: `v1.0.0`. Prefer installing from a tagged release when you need a stable pin.

More detail: https://docs.myshopie.com/ai-agents/
