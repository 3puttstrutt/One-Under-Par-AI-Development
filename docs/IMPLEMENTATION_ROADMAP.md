# One Under Theme Implementation Roadmap

This roadmap covers work remaining after the foundation fixes on `agent/one-under-premium-audit`. Production must remain untouched until preview QA, merchant approval, and a documented launch decision are complete.

## Phase 1: Foundation and critical fixes

### Completed on this branch

- Extract and validate the Shopify export while retaining the original ZIP.
- Establish the development branch and baseline extraction commit.
- Correct strict JSON, section schema, and password-layout defects.
- Restore visible keyboard focus even when PageFly styles are present.
- Add reduced-motion safeguards and consistent main/skip landmarks.
- Replace link-like controls with semantic buttons and expose control state.
- Correct search and product quantity semantics.
- Make Organization structured data store-driven and remove duplicate breadcrumb schema.
- Remove unsupported generated claims and duplicate conversion sections from active templates.
- Replace missing homepage assets with responsive Shopify image settings and placeholders.
- Replace mobile-navigation and product-card action links with semantic controls, synchronize drawer state/focus, and remove duplicate responsive-gallery IDs.
- Confirm the target Shopify theme is unpublished and conduct an initial rendered mobile preview audit.
- Sync the reviewed files to the unpublished **One Under Par – AI Development** theme and complete post-sync checks for home, collection, PDP, search, cart, mobile navigation, and 404 flows.

### Remaining before feature work

- Install a current Ruby/Shopify CLI toolchain and run Theme Check.
- Run keyboard, VoiceOver, axe, 200%/400% zoom, reflow, and touch-target tests.
- Establish mobile Lighthouse baselines for home, collection, product, search, and cart.
- Correct the literal Terms placeholders, duplicate contact-policy block, and shipping-market conflict after merchant/legal approval.
- Confirm whether PageFly is still used; remove its layouts/assets only after dependency review.
- Consolidate active custom inline CSS into versioned assets.

Exit criteria: no Critical code defects; no serious automated accessibility findings; approved baseline performance measurements; all active templates render without console/Liquid errors.

## Phase 2: Conversion and merchandising

- Create product metafield definitions for approved fit, profile, construction, materials, care, and merchandising copy.
- Build a merchant-controlled PDP details section that renders only populated metafields.
- Create and assign an approved fit-guide page to the variant picker.
- Validate sticky add-to-cart across variants, sold-out products, dynamic checkout, and mobile safe areas.
- Configure Shopify Search & Discovery filters, synonyms, boosts, and complementary products.
- Normalize product options and filter labels across the catalog.
- Curate related products and a restrained cart cross-sell set.
- Verify cart free-shipping progress against the approved threshold, currency, market, and exclusions.
- Test search no-results recovery, predictive search, spelling variants, and high-value queries.
- Review product-card option density and quick buy with analytics before changing defaults.

Exit criteria: all product claims are data-backed and merchant-approved; search/filter tasks succeed on mobile; cart and sticky purchase flows pass variant/error testing.

## Phase 3: Brand experience

- Upload approved homepage editorial images into Shopify and configure meaningful alt text.
- Move custom English copy into locale keys or locale-aware content sources.
- Approve and centralize philanthropy, beneficiary, shipping, return, and review language.
- Refine homepage hierarchy using one hero, one collection-discovery path, proof backed by real data, and one email-capture moment.
- Build distinct footer navigation for shopping, support, company, and legal destinations.
- Confirm social profiles, share images, favicon, store description, and organization details.
- Approve and configure the Klaviyo first-order offer, consent copy, timing, and frequency cap.
- Decide whether supported markets require country/language selectors.
- Remove deprecated custom sections after merchant sign-off and version history confirmation.

Exit criteria: all brand/policy content is approved; no hard-coded unsupported claims remain active; imagery and copy are localized for supported markets.

## Phase 4: Testing and launch readiness

- Run Theme Check, strict JSON/schema validation, and a broken-link crawl.
- Test Chrome, Safari, and Firefox plus current iOS Safari and Android Chrome.
- Test keyboard-only navigation, VoiceOver, focus order/return/obscuring, dialogs, filters, menus, cart drawer, and variant changes.
- Run axe and manual WCAG 2.2 AA checks, including contrast and 400% zoom/reflow.
- Run Lighthouse and WebPageTest on representative catalog pages; set budgets for LCP, CLS, INP, JavaScript, CSS, fonts, and apps.
- Validate Product, Organization, Website, Article, and Breadcrumb structured data on rendered URLs.
- Validate Open Graph/Twitter previews.
- Audit Customer Events pixels, consent behavior, and duplicate analytics/purchase events.
- Test Klaviyo, Loox, Notify Me, Search & Discovery, dynamic checkout, policies, discounts, gift cards, localization, and error states.
- Complete stakeholder UAT on the development theme and document rollback steps.
- Keep subsequent changes on the development theme and re-test them there; only after explicit merchant approval should a separate production publication action be scheduled.

Exit criteria: zero Critical/High launch blockers; merchant UAT approval; documented performance/accessibility results; confirmed analytics; rollback plan; explicit authorization for any production action.

## Shopify admin access required

- Theme preview/development-theme sync and Theme Inspector access.
- Navigation, policies, Search & Discovery, metafields, markets, languages, files, and product data.
- App embed and app configuration review.
- Customer Events/pixels, consent, checkout, and analytics verification.

## Guardrails

- Never merge or publish automatically.
- Never edit the live theme during development.
- Never add urgency, scarcity, reviews, guarantees, discounts, shipping promises, policies, beneficiaries, or product claims without authoritative merchant data.
- Keep content merchant-editable and sections compatible with the theme editor.
- Prefer Shopify-native capabilities and progressive enhancement over new libraries or apps.
