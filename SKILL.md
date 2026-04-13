---
name: price-compare-browser
version: "1.2.0"
description: "Use this skill when the user wants to compare prices for any product across websites using the browser, find the cheapest listing, compare product variants such as color, storage, size, model, pack count, or region, check shipping or delivery costs, and verify which offer is actually best after visible fees, discounts, and retailer terms. Prefer a fixed retailer set first: Noon.com, Amazon.com, Amazon.sa, Jarir.com, and Alephksa.com."
---

# Price Compare Browser

Use this skill to research product pricing across multiple online stores and present a practical comparison.

## Purpose

Find the best buying option for a product the user names by browsing current listings, extracting comparable details, and comparing total purchase value rather than headline price alone.

## Preferred Retailers

Start with this retailer set unless the user asks otherwise:

1. Noon.com
2. Amazon.com
3. Amazon.sa
4. Jarir.com
5. Alephksa.com

Use these sites as the default comparison pool for every product request.

If one or more of these sites has no relevant listing, is out of stock, or lacks enough detail to confirm the product match, expand to other reputable retailers only after checking the preferred set.

If the user says "compare everywhere" or names other stores, keep the preferred set and then add the requested stores.

## Site Search Rules

Search each preferred retailer directly with site-specific queries.

Examples:

- site:noon.com iPhone 15 Pro
- site:amazon.sa iPhone 15 Pro
- site:jarir.com iPhone 15 Pro
- site:alephksa.com iPhone 15 Pro
- site:amazon.com iPhone 15 Pro

Prefer direct product pages over category pages, blog pages, or search result pages.

## Inputs to Gather

Collect the minimum details needed before searching:

- Product name.
- Exact model number or SKU when available.
- Variant details: color, storage, size, capacity, quantity, region, edition, connectivity, or condition.
- User location if shipping, taxes, or store availability matter.
- Preferred retailers or retailers to avoid.
- Whether the user wants the lowest upfront price, fastest delivery, best warranty, or best overall value.

## Ambiguous Product Rule

If the product name can refer to multiple variants, do not assume all variants are comparable.

Examples:

- iPhone 15 Pro can vary by storage, color, region, carrier lock status, dual-SIM setup, and condition.
- Shoes can vary by size, width, and color.
- TVs can vary by screen size and regional model suffix.
- Groceries and supplements can vary by pack count and unit size.

When the user gives a broad product name such as "iPhone 15 Pro", follow this rule:

1. Identify the main variant dimensions that materially affect price.
2. Ask one concise clarification question that bundles the important choices.
3. If the user does not care about some dimensions, default to comparing the cheapest in-stock exact new variant and state that assumption.

Suggested clarification pattern:

"Do you want a specific storage, color, condition, and country version, or should I compare the cheapest new unlocked iPhone 15 Pro offers across common variants?"

## Variant Priority

For phones, laptops, and consumer electronics, prioritize these fields before comparing price:

1. Exact model family.
2. Storage or capacity.
3. New, refurbished, or used condition.
4. Region or country version.
5. Carrier locked or unlocked.
6. Color, only if the user cares or pricing differs.

Color usually matters less than storage or condition, but it should still be preserved when the user specifies it.

## Search Workflow

1. Check the preferred retailer set first: Noon.com, Amazon.com, Amazon.sa, Jarir.com, and Alephksa.com.
2. Search the product on each site using the browser.
3. Open the most relevant listings and confirm the exact product match before comparing.
4. Extract the fields that affect real purchase cost and comparability.
5. If listings split into materially different variants, group them by variant before ranking.
6. Remove mismatched products, bundles, refurbished units, subscription prices, member-only prices, preorder listings, and out-of-stock offers unless the user asked for them.
7. If the preferred retailer set is insufficient, expand to other reputable retailers.
8. Present the best options in a compact comparison table, then recommend the best choice based on the user's goal.

Always compare at least 3 valid sources when possible. Prefer official stores and established retailers before low-trust sellers.

## Data to Capture Per Listing

For each valid listing, capture:

- Retailer or marketplace name.
- Product title.
- Confirmed model or variant.
- Item price.
- Shipping cost or delivery fee.
- Coupon, auto-applied discount, or bundle condition.
- Estimated delivery time.
- Stock status.
- Seller type: official store, marketplace seller, or third party.
- Return policy or warranty details when clearly shown.
- Product URL.

When possible, compute total visible cost as:

Price + shipping - visible discount

If taxes are unknown, state that totals are pre-tax.

## Matching Rules

Only compare listings that are truly comparable.

Treat these as different products unless the user says otherwise:

- Different model numbers.
- Different storage or memory sizes.
- Different pack counts or quantities.
- Refurbished vs new.
- International vs local version.
- Locked vs unlocked.
- Different warranty coverage.
- Bundled vs standalone offers.

If exact matches are scarce, say so clearly and provide the closest alternatives in a separate section.

## Ranking Logic

Rank offers by the user's objective:

- Lowest cost: total visible cost first, then delivery speed, then seller trust.
- Fastest delivery: delivery speed first, then total visible cost.
- Best value: balance total cost, seller trust, warranty, and delivery.
- Trusted seller only: prioritize official brand stores or major retailers.

Do not call something the best deal if key terms are missing or the listing may not be an exact match.

## Output Format

Structure the response in this order:

1. One-sentence answer naming the current best option.
2. A short assumption line if the product request was ambiguous.
3. A markdown table with the top listings.
4. Two to four bullets explaining tradeoffs.
5. A short note on uncertainty, such as taxes, region, or changing stock.

Use a table with these columns when available:

| Retailer | Match | Item Price | Shipping | Total Visible Cost | Delivery | Stock | Notes |
|---|---|---:|---:|---:|---|---|---|

If variant ambiguity is important, include the Match column in this style:

- iPhone 15 Pro, 256GB, Black, unlocked, new, UAE version
- iPhone 15 Pro, 128GB, Blue, unlocked, refurbished

Keep the table compact. Use the product page title only when it helps disambiguate the model.

## Quality Rules

- Prefer current live product pages over blog posts or deal roundups.
- Prefer first-party or major retailers over scraped aggregator pages.
- Double-check currency.
- Convert currencies only if needed, and label the conversion clearly.
- Do not invent shipping, tax, coupon, or warranty details.
- Mention when a price depends on membership, trade-in, financing, or subscription.
- Mention when marketplace sellers vary and the result may change.
- If all sources are weak or inconsistent, say the result is provisional.

## Safety Checks

Stop and clarify before recommending when:

- Multiple similar models appear and the exact model is unclear.
- The user asks for medicine, age-restricted items, or regulated products with location-sensitive rules.
- The only low prices come from suspicious or low-trust sellers.
- The price difference depends on hidden conditions not visible on the page.
- A high-price-impact variant such as storage, condition, or region was not specified.

## Example Behavior

User: Compare prices for iPhone 15 Pro.

Agent behavior:

- Recognize that the request is ambiguous.
- Ask a single bundled clarification question covering storage, condition, region, and locked vs unlocked status.
- Start with Noon.com, Amazon.com, Amazon.sa, Jarir.com, and Alephksa.com after the user clarifies.
- If the user says "any color, cheapest new unlocked", compare only new unlocked listings and group by storage.
- Recommend the best option within the chosen variant scope.

User: Compare prices for Sony WH-1000XM6 in black.

Agent behavior:

- Search the preferred retailer set first.
- Match only Sony WH-1000XM6, black, new condition.
- Exclude older WH-1000XM5 listings.
- Compare visible total cost, delivery estimate, and seller trust.
- Recommend the best option with a concise table and caveat if taxes are not included.

## Style

Be concise, practical, and skeptical of incomplete deals. Optimize for decision usefulness, not raw data volume.
