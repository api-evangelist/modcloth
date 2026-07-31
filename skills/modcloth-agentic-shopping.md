---
name: Shop Modcloth as an agent (UCP)
description: >-
  Search Modcloth's catalog, build a cart, and run a buyer-approved checkout
  through the Universal Commerce Protocol (UCP) MCP endpoint. For personal
  shopping assistants transacting on a buyer's behalf.
api: mcp/modcloth-mcp.yml
transport: mcp
endpoint: https://modcloth.com/api/ucp/mcp
operations: [search_catalog, lookup_catalog, create_cart, create_checkout, update_checkout, complete_checkout]
source: https://modcloth.com/agents.md
---

# Shop Modcloth as an agent

Modcloth (https://modcloth.com) is a Shopify store that implements the
Universal Commerce Protocol (UCP) over MCP. Use this skill to shop and buy
on a buyer's behalf.

## Prerequisites
- Confirm capabilities: `GET https://modcloth.com/.well-known/ucp`.
- MCP endpoint: `POST https://modcloth.com/api/ucp/mcp` with
  `Content-Type: application/json`. Supply your UCP agent profile URI
  (the endpoint returns `invalid_profile_url` without one).
- Pass `context.address_country` and `context.currency` for accurate
  pricing and availability.

## Steps
1. **Discover** — `GET /.well-known/ucp` to confirm supported versions and
   capabilities.
2. **Search** — call `search_catalog` with the buyer's intent to find products.
   Use `lookup_catalog` to resolve a specific product/variant.
3. **Cart** — call `create_cart` to add the chosen items.
4. **Checkout** — call `create_checkout` to start the purchase.
5. **Fulfill** — call `update_checkout` to set shipping address, shipping
   method, and any discount.
6. **Complete** — call `complete_checkout` to finalize.

## Rules
- **Buyer approval is mandatory.** Never complete payment without explicit,
  contemporaneous buyer consent. If you cannot get approval at the moment of
  payment, route through Shop Pay via the Shop skill
  (https://shop.app/SKILL.md) instead of scripting the store directly.
- **Back off on 429.** The MCP endpoint is rate-limited per IP.
- **Read-only browsing needs no auth** — use `GET /products/{handle}.json`,
  `GET /collections/{handle}/products.json`, and
  `GET /search?q={query}&type=product` when you only need to read catalog data.

See also: conventions/modcloth-conventions.yml, authentication/modcloth-authentication.yml.
