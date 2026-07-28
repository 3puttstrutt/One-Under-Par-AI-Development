# One Under Shopify Theme Audit

Audit date: 2026-07-27

Repository: `3puttstrutt/One-Under-Par-AI-Development`

Branch: `agent/one-under-premium-audit`

Theme: Clean Canvas **Symmetry 8.1.0**

Scope: extracted development-theme source only. No production theme was accessed, modified, published, or deployed.

## Executive summary

The repository originally contained a Shopify export ZIP and a README. The export has been safely extracted into the repository root, the original ZIP remains intact, and the required Shopify directories and key files are present.

Symmetry provides a solid Online Store 2.0 base with responsive product media, native predictive search, faceted filtering, a cart drawer, sticky add-to-cart, localization scaffolding, and Shopify-generated product structured data. The largest risks were in custom One Under layers added over the base theme: active pages depended on missing image assets and external CSS backgrounds, several generated merchandising sections inferred unsupported product claims, PageFly styles suppressed focus indicators, two templates were not strict JSON, a section schema was invalid, and the password layout emitted invalid markup.

This branch fixes the critical foundation issues and keeps unsupported generated sections out of active templates. Remaining work requires merchant-approved content, Shopify admin configuration, real catalog data, and storefront testing.

## Repository and architecture

- Standard folders confirmed: `assets`, `config`, `layout`, `locales`, `sections`, `snippets`, `templates`.
- Required files confirmed: `layout/theme.liquid`, `config/settings_schema.json`.
- Original archive preserved: `theme_export__www-oneunder-com-one-under-par-ai-development__27JUL2026-0726pm.zip`.
- Online Store 2.0 JSON templates and section groups are present.
- Global payload baseline: `assets/main.css` is about 267 KB and `assets/main.js` is about 148 KB before transfer compression.
- Enabled app embeds in exported settings: Klaviyo, Loox, and Notify Me. PageFly and Essential Trust Badges embeds are disabled, though PageFly theme files remain.

## Status definitions

- **Implemented**: changed and statically validated on this branch.
- **Existing / acceptable**: reviewed and no immediate code change is justified.
- **Remaining**: safe follow-up engineering work.
- **Merchant input**: content, policy, catalog, or brand decision is required.
- **Shopify admin**: requires theme preview, app configuration, store data, or admin access.

## Findings

| Area | Current problem | Customer or business impact | Severity | Recommended solution | Files affected | Implementation status |
|---|---|---|---|---|---|---|
| Repository repair | Theme source existed only inside a ZIP, preventing normal review and version control. | Changes could not be reviewed, tested, or safely deployed as source. | Critical | Extract into the repository root, validate required folders/files, and retain the archive. | All theme folders; original ZIP | **Implemented** |
| Theme architecture | Custom One Under sections duplicate or override base-theme behavior with large inline CSS blocks. | Higher regression risk, repeated styles, and slower maintenance. | High | Keep base Symmetry primitives active; progressively move approved custom CSS into scoped assets. | `sections/oup-*.liquid` | **Remaining** |
| Unsupported merchandising | Custom PDP logic inferred fit, breathability, use cases, and other claims from product handles/titles. | Incorrect claims can reduce trust and create compliance risk. | Critical | Remove the generated section from active templates; later drive approved claims from product metafields. | `templates/product.json`, `sections/oup-product-proof.liquid` | **Implemented** for active template; metafield rebuild **Remaining** |
| Duplicate conversion content | Homepage stacked editorial, collection, and a second long-form conversion section; a separate CSS-only section hid selected blocks. | Duplicate headings, excessive page length, brittle display logic, and conflicting messages. | High | Keep one coherent editorial flow and remove CSS-dependent duplicate sections from the active template. | `templates/index.json`, `sections/oup-conversion-home.liquid`, `sections/oup-lifestyle-sources.liquid` | **Implemented** for active template |
| Collection merchandising | Custom collection funnel used hard-coded handles, copy, and product-construction claims. | Broken links or inaccurate recommendations when the catalog changes. | High | Use native collection content and filters now; rebuild recommendations with collection settings/metafields if approved. | `templates/collection.json`, `sections/oup-collection-funnel.liquid` | **Implemented** for active template |
| Homepage imagery | Four referenced `oup-lifestyle-*.jpg` assets did not exist; external CSS backgrounds masked the failure. | Broken/duplicated requests, missing semantic image content, unstable third-party delivery, and poor LCP control. | Critical | Add Shopify image-picker settings, responsive widths, stable dimensions, and placeholders until approved assets are uploaded. | `sections/oup-editorial-intro.liquid`, `templates/index.json` | **Implemented**; image uploads require **Merchant input / Shopify admin** |
| Liquid/schema correctness | Map section schema had a trailing comma. | Theme editor or section parsing could fail. | Critical | Correct the JSON schema and validate every section schema. | `sections/map.liquid` | **Implemented** |
| JSON templates | Homepage and collection templates contained comment headers and were not strict JSON. | Generic validators and deployment pipelines fail even if Shopify tolerates generated comments. | High | Store templates as strict JSON. | `templates/index.json`, `templates/collection.json` | **Implemented** |
| Markup correctness | Password layout could output two `class` attributes on `<body>`. | Invalid DOM and inconsistent animation styling. | High | Merge conditional classes into one attribute. | `layout/password.liquid` | **Implemented** |
| Landmarks and skip navigation | PageFly layout lacked a main landmark and skip link; password main had no skip target. | Keyboard and screen-reader users take longer to reach page content. | High | Add a skip link and consistent `main#content` landmark. | `layout/theme.pagefly.liquid`, `layout/password.liquid` | **Implemented** |
| Keyboard focus | Theme JavaScript conditionally removes outlines and PageFly CSS explicitly suppresses `:focus-visible`. | Keyboard users can lose their position, violating WCAG 2.2 AA focus requirements. | Critical | Load a final accessibility stylesheet with visible focus safeguards, including a light outline on dark regions. | `assets/accessibility.css`, all layouts | **Implemented** |
| Reduced motion | Base theme only disables motion on selected `.has-motion` elements. Custom sections still animate transforms. | Motion-sensitive users can experience discomfort. | High | Apply a global reduced-motion override for animations, transitions, and smooth scrolling. | `assets/accessibility.css` | **Implemented** |
| Semantic controls | Filter drawers, layout switches, page shade, and product quantity controls used `href="#"` links as buttons. | Unexpected URL changes, confusing semantics, and unreliable assistive-technology behavior. | High | Use real buttons and preserve event delegation. | `layout/theme.liquid`, `snippets/faceted-filters.liquid`, `sections/main-collection.liquid`, `sections/main-search.liquid`, product sections | **Implemented** |
| Control state | Filter and mobile grid controls did not expose expanded/pressed state. | Screen-reader users cannot determine current UI state. | High | Add `aria-expanded`/`aria-pressed` and update them in the existing delegated JavaScript. | `assets/main.js`, collection/search sections | **Implemented** |
| Quantity input | Product quantity accepted values below one and increment/decrement links had no useful navigation fallback. | Invalid purchase attempts and confusing interaction. | Medium | Use buttons and add `min="1"` plus numeric input hints. | `sections/main-product.liquid`, `sections/featured-product.liquid` | **Implemented** |
| Search semantics | Search forms lacked a search landmark; inputs used generic text keyboards. | Weaker navigation and slower mobile entry. | Medium | Add `role="search"`, `type="search"`, and `enterkeyhint="search"`. | `sections/header.liquid`, `sections/main-search.liquid` | **Implemented** |
| Search status and empty state | Result count was not announced and the empty state used an out-of-order heading. | Dynamic results may be missed by assistive technology; heading structure becomes noisy. | Medium | Add a polite status region and use status text for no results. | `sections/main-search.liquid` | **Implemented** |
| Product structured data | Shopify's `product | structured_data` filter is used, which is the correct catalog-aware source. | Good baseline eligibility for product rich results. | Low | Retain it; validate rendered output against real variants and reviews before launch. | `snippets/structured-data-product.liquid` | **Existing / acceptable**; rendered validation requires **Shopify admin** |
| Organization structured data | Organization name, email, and description were hard-coded; logo depended on section context. | Metadata could disagree with Shopify business data. | High | Use `shop.name`, `shop.email`, `shop.description`, configured header logo, and configured social URLs. | `snippets/structured-data-header.liquid` | **Implemented** |
| Breadcrumb structured data | Global custom breadcrumbs and visual PDP breadcrumbs could emit two `BreadcrumbList` objects. | Search engines receive redundant/conflicting trails. | Medium | Keep one global structured breadcrumb source and suppress the duplicate in the visual snippet. | `snippets/oup-structured-breadcrumbs.liquid`, `snippets/breadcrumbs.liquid` | **Implemented** |
| Canonicals and robots | Canonical URLs are present; cart, search, customer, password, and 404 pages are noindexed. | Sound baseline crawl control. | Low | Retain; test pagination/filter canonical behavior on the development preview. | `snippets/doc-head-core.liquid` | **Existing / acceptable**; preview verification **Remaining** |
| Open Graph/social | OG and Twitter tags exist with page imagery when available. Image metadata reports original dimensions while requesting a 1200px derivative. | Mostly functional, but validators may see inconsistent width/height metadata. | Medium | Compute derivative dimensions and test representative product/article/home shares. | `snippets/doc-head-social.liquid` | **Remaining** |
| SEO copy | Global metadata and several custom sections contain fixed brand, philanthropy, shipping, and return statements. | Stale or unapproved claims may appear in search and social results. | High | Merchant must approve each claim and move reusable copy into theme settings or Shopify content. | `snippets/doc-head-core.liquid`, `snippets/doc-head-social.liquid`, `sections/oup-*.liquid`, group JSON | **Merchant input** |
| Responsive images | Base image snippet generates `srcset`, `sizes`, dimensions, and loading behavior. Some custom/raw images bypass it. | Avoidable bytes and CLS on custom content. | High | Use Shopify image pickers and `image_url`/`image_tag`; migrate the remaining Shopify Files logo in the custom homepage section. | `snippets/image.liquid`, `sections/oup-editorial-intro.liquid`, `sections/oup-conversion-home.liquid` | Core homepage **Implemented**; remaining inactive section **Remaining** |
| LCP | Homepage hero is eager/high priority after a Shopify image is configured; currently it falls back to a placeholder because brand assets are absent from the repo. | Preview cannot represent final LCP until a real image is configured. | High | Upload a correctly cropped source, keep one high-priority hero, and measure mobile LCP. | `sections/oup-editorial-intro.liquid` | Code **Implemented**; asset and measurement require **Shopify admin** |
| CLS | Most base images have dimensions. External backgrounds, app widgets, review blocks, and dynamic payment buttons can still shift. | Layout movement can reduce conversion and Core Web Vitals scores. | High | Reserve app/widget space after observing actual renders; keep intrinsic image dimensions. | Product template, app embeds, custom sections | **Shopify admin / Remaining** |
| JavaScript performance | Global `main.js` is about 148 KB and app code arrives through `content_for_header`; feature assets are generally deferred. | Parse/execute cost can affect mobile INP. | Medium | Profile on a real preview, retain deferred feature loading, and disable unused app embeds before launch. | `layout/theme.liquid`, `assets/main.js`, app settings | **Remaining / Shopify admin** |
| CSS performance | Global `main.css` is about 267 KB; custom sections add substantial inline CSS; PageFly CSS remains in the repository. | More transfer, style calculation, and cache fragmentation. | Medium | Remove unused PageFly assets only after confirming no PageFly templates; consolidate active custom section styles. | `assets/main.css`, `assets/pagefly-*`, `snippets/pagefly-*`, `sections/oup-*.liquid` | **Remaining / Shopify admin** |
| Font loading | Theme preloads base and heading fonts and uses `font-display: fallback`. Four custom font faces may resolve to repeated Inter files. | Extra preload/download work and possible late typography changes. | Medium | Confirm resolved font URLs, preload only above-fold faces, and test `fallback` versus `swap` with field data. | All layouts, `snippets/doc-head-styles.liquid` | **Remaining** |
| Header/mobile navigation | Symmetry supplies sticky navigation, mobile drawer behavior, search, accessible names, and menu editor settings. No complete keyboard/voice test was possible without rendering. | Navigation defects have site-wide conversion impact. | High | Test menu opening, submenu traversal, Escape, focus return, zoom, and 320px layouts in preview. | `sections/header.liquid`, `snippets/main-nav-links.liquid`, `assets/main.js` | Code improvements **Implemented**; end-to-end test requires **Shopify admin** |
| Collection filtering/sorting | Native filters, counts, sort, sticky filtering, and availability options are configured. Their quality depends on Search & Discovery data. | Poor option naming or empty filters can obstruct discovery. | Medium | Configure standardized product options/metafields and test filter combinations with real catalog data. | `sections/main-collection.liquid`, `snippets/faceted-filters.liquid` | UI semantics **Implemented**; data requires **Shopify admin** |
| Product cards | Cards use responsive images and expose prices/options; quick buy adds interaction and JavaScript cost. | Good merchandising baseline, but option density may overwhelm mobile cards. | Medium | A/B test option visibility and quick buy after analytics are available; verify card alt text and sold-out behavior. | `snippets/product-block.liquid`, `config/settings_data.json` | **Remaining / Shopify admin** |
| Variant selection | Native radio/select patterns, availability, URL updates, and price status are present. Fit guide is disabled because no page is assigned. | Customers lack a centralized fit reference; variant announcements still need screen-reader testing. | High | Create an approved fit-guide page and assign it; test unavailable combinations and focus/announcement behavior. | `snippets/variant-picker.liquid`, `templates/product.json` | **Merchant input / Shopify admin** |
| Product purchase flow | Sticky add-to-cart is enabled on mobile and desktop; dynamic checkout and quantity are active. | Strong baseline for purchase access. | Medium | Verify sticky bar does not cover content, variant selection is respected, and dynamic payment buttons do not shift. | `templates/product.json`, `snippets/sticky-atc.liquid`, `assets/sticky-atc.js` | Quantity semantics **Implemented**; preview test **Remaining** |
| Cart drawer | Drawer contains subtotal, policy-aware tax/shipping copy, order notes, free-shipping progress, cross-sell capability, and empty-state settings. | Strong baseline; cross-sells and policy text depend on configuration. | Medium | Populate only approved cross-sells, verify focus trap/return, test errors, and confirm threshold matches policy. | `sections/cart-drawer.liquid`, `snippets/cart-item.liquid`, `snippets/free-shipping-bar.liquid` | **Shopify admin / Remaining** |
| Shipping and returns | Exported configuration states free shipping over $100 and easy/free domestic returns in multiple places. The repository cannot prove the policies. | A mismatch creates support cost and trust/compliance risk. | Critical | Merchant must verify threshold, geography, exclusions, return fees, window, and exact wording against Shopify policies. | Header/footer group JSON, custom sections, `config/settings_data.json` | **Merchant input** |
| Trust and reviews | Loox is enabled and custom copy references verified reviews; Essential Trust Badges is disabled. | Review claims must match actual Loox moderation/verification behavior; app load affects performance. | High | Confirm Loox configuration and claim wording; avoid separate badge apps unless measured benefit exceeds cost. | `config/settings_data.json`, `templates/product.json`, custom sections | **Shopify admin / Merchant input** |
| Cross-sells and upsells | Related products are active; cart cross-sells exist but require selected products. | Useful discovery without fabricated urgency, but relevance depends on merchandising. | Medium | Use Search & Discovery recommendations and a small merchant-curated cart set. | `sections/related-products.liquid`, `sections/cart-drawer.liquid` | **Shopify admin** |
| Email capture | Klaviyo embed and footer newsletter are enabled; inactive custom homepage code contains another signup form. | Multiple active forms could duplicate tracking or messaging. | Medium | Keep one primary capture per page, verify consent language and Klaviyo event flow by market. | `config/settings_data.json`, `sections/footer.liquid`, inactive `sections/oup-conversion-home.liquid` | Active-template duplication reduced; **Shopify admin** verification |
| Footer architecture | Footer has brand text, navigation, newsletter, social links, policy-linked promos, payment icons, and localization controls disabled. | Good baseline, but duplicate menu assignment and claims require review. | Medium | Create distinct footer/legal menus, verify every link, and enable selectors only for supported markets/languages. | `sections/footer-group.json`, `sections/footer.liquid` | **Merchant input / Shopify admin** |
| Localization | Eight locale files exist, but custom One Under sections are hard-coded in English. | Non-English storefronts receive mixed-language content. | High | Move approved custom copy to locale keys or locale-aware dynamic sources before enabling additional languages. | `locales/*.json`, `sections/oup-*.liquid` | **Remaining / Merchant input** |
| Analytics readiness | No custom data-layer contract is defined in source; app pixels may be injected through Shopify. | Funnel analysis and experimentation may be inconsistent. | Medium | Inventory Customer Events pixels, define event ownership, test consent mode, and avoid duplicate page/purchase events. | Shopify admin pixels, app embeds, optional custom events | **Shopify admin** |
| App performance | Klaviyo, Loox, and Notify Me are enabled; PageFly files remain despite its embed being disabled. | Third-party scripts can degrade LCP/INP and create duplicate UI. | High | Measure each app on preview, remove unused embeds/assets only after dependency confirmation, and set an app performance budget. | `config/settings_data.json`, PageFly files, `content_for_header` | **Shopify admin / Remaining** |
| Error/empty states | Search and cart have empty states; product add errors use an alert. Network, predictive-search empty results, and unavailable-variant flows still need rendered testing. | Failure states may trap customers or provide unclear recovery. | Medium | Test offline/slow requests, no results, invalid quantity, sold out, and cart API errors; keep messages actionable. | Search/cart/product JS and Liquid | Search empty state **Implemented**; broader testing **Remaining** |
| Color contrast | Exported core palette appears high contrast, but overlays, images, app widgets, and merchant-selected colors were not render-tested. | WCAG contrast may fail on real imagery or app content. | High | Run axe plus manual contrast checks on representative preview pages and every color scheme. | Theme settings, custom sections, app widgets | **Shopify admin / Remaining** |
| WCAG 2.2 AA validation | Static fixes address major focus and semantic issues, but no rendered browser/assistive-technology test was available. | Compliance cannot be claimed from source review alone. | Critical | Test keyboard, VoiceOver, zoom/reflow, target size, errors, focus order/obscuring, and automated axe across key templates. | Entire theme | **Remaining / Shopify admin** |

## Implemented in this branch

- Extracted and validated the complete theme source while preserving the ZIP.
- Added global focus-visible and reduced-motion safeguards after theme/app styles.
- Corrected invalid password markup and PageFly/password landmark gaps.
- Replaced link-like UI controls with semantic buttons and synchronized UI state.
- Improved product quantity and search semantics.
- Corrected the invalid map schema and strict JSON templates.
- Made Organization schema use Shopify store data.
- Removed duplicate breadcrumb structured data.
- Replaced missing homepage asset dependencies with merchant-configurable Shopify responsive images and safe placeholders.
- Removed unsupported generated PDP, collection, and duplicate homepage sections from active templates.

## Validation performed

- Confirmed all required theme folders and key files.
- Confirmed base theme identity/version from `settings_schema.json`.
- Parsed every `.json` file with `jq`.
- Extracted and parsed every section `{% schema %}` block with `jq`.
- Checked all `asset_url` references against files in `assets`.
- Ran `git diff --check`.
- Attempted Shopify Theme Check. It could not run because the available system Ruby is 2.6.10 while current Theme Check dependencies require Ruby 2.7+ (and current Nokogiri releases require newer Ruby). No system packages were changed.

## Testing not possible without Shopify access

- Theme-editor schema rendering and section setting migration.
- Live Liquid rendering with products, variants, collections, localization, policies, and apps.
- Lighthouse/Core Web Vitals on authenticated development-theme preview URLs.
- axe, keyboard, VoiceOver, zoom/reflow, and focus-obscuring checks on rendered pages.
- Rich Results Test and social share debugger checks against public previewable URLs.
- Checkout, Customer Events pixels, consent, Klaviyo, Loox, Notify Me, and Search & Discovery configuration.
- Push/deploy to the named development theme. No credentials were provided and no deployment was attempted.

## Merchant decisions required

1. Upload and approve the four homepage editorial images and their alt text.
2. Approve every philanthropy, beneficiary, donation amount, family-owned, shipping, returns, product-performance, and review-verification statement.
3. Confirm shipping threshold/geography/exclusions and full return terms.
4. Provide an approved fit guide and product-specific merchandising fields/metafields.
5. Confirm footer/legal menu structure, supported markets, languages, and newsletter consent copy.
6. Decide whether PageFly is still used anywhere and which app embeds are essential.
