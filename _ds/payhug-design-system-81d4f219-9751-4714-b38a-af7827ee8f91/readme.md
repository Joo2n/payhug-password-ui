# PayHug Design System (페이허그 디자인 시스템)

A design system for **PayHug (페이허그)** — a Korean fintech that gives small merchants (가맹점주) their card and delivery-app sales **the day before they would normally settle**. The core product is **미리 받는 돈** ("money received in advance" / 선정산, early-settlement): PayHug fronts yesterday's net sales — card revenue minus card fees, delivery revenue minus platform fees — and reconciles the small difference against the real settlement on following days.

This repo lets a design agent build PayHug-branded interfaces (mobile merchant app, desktop settlement admin, marketing, slides) against the real tokens, components, and screen recreations.

## Sources

This system was reconstructed from a prototype repo that a prior Claude account extracted from PayHug's Figma:

- **GitHub:** https://github.com/Joo2n/payhug-prototype — `PayHug 앱 프로토타입 — Claude Design`. Tokens (`tokens/*.css`), the compiled component bundle, and the `payhug_app` + `cashnote` UI kits live there. Explore it to go deeper on screen inventory and copy.
- **Figma (referenced, not bundled):** `PH_화면설계서.fig` / `수정.fig` — the source of truth for variables and screens. We do not have direct access; values here were lifted from the prototype, which was itself derived from the Figma.

The prototype committed only the *compiled* bundle, so every component below was re-authored as editable `.jsx` + `.d.ts` source from that bundle. If you have access to the Figma, prefer it for any net-new screen.

> **Note on the two layers.** The GitHub prototype is a previous extraction. This project is the clean, editable design system rebuilt from it — edit components and tokens here, and consuming projects pick up the changes.

---

## CONTENT FUNDAMENTALS

PayHug copy is **Korean, plain-spoken, and money-first**. It exists to make a merchant trust a number.

- **Voice / register:** friendly-but-precise **해요체** (`반영돼요`, `포함돼요`, `없어요`). Never the stiff `합니다` register, never marketing hype, never exclamation marks, **never emoji**.
- **Person:** addresses the merchant implicitly ("어제 매출액", "미리 받는 돈") — rarely uses 당신/고객님. The product refers to itself as **페이허그**.
- **Explain the "why" of every number.** Notes are short paragraphs that justify a figure: *"선정산은 예상 카드 수수료율을 기준으로 지급돼요. 실제 정산 후 발생한 차액은 다음 선정산 금액에 자동으로 반영돼요."* These render in the green note block (`--ph-green-tint` fill, forest text).
- **Domain labels are exact and terse.** 미리 받는 돈 · 어제 매출액 · 매입기준 매출 · 선정산 제외액 · 미매입 / 매입 완료 · 이체완료 · 예정일 경과 · 예상 지급 차액 · 차감액 · 전산 수수료(면제). Use these verbatim — they are regulated/accounting terms, not free copy.
- **Casing & numerals:** Korean labels stay as-is; Latin/brand uses `Payhug` / `PayHug Admin`. **All money is `1,234,567원`** with comma grouping and a trailing `원`, set in Inter tabular figures (`.ph-num`) so columns align. Negative/fee amounts are coral; positive net/profit is mint-green.
- **Buttons:** action verbs — `바로이체`, `미리보기`, `내역 더 불러오기`, `미매입 내역 보러가기`, `계좌 입금 내역보기`.
- **Dates:** UI shows `26년 6월 24일 (수)` or `2026.06.24`; an "updated at" caption reads `최종 업데이트 2026년 06월 24일 11:30`.

---

## VISUAL FOUNDATIONS

A calm, data-dense fintech surface. Two faces share one palette: a **green brand layer** (marketing, app header, primary CTAs) and a **blue/green/coral functional layer** (the merchant + admin data UI).

- **Color.**
  - **Brand greens** carry identity: deep `--ph-forest #163300` ink and the bright `--ph-green #9FE870` primary accent (with forest-green text on it). Green tints (`--ph-green-tint`) wash explanatory note blocks.
  - **Blue is the UI primary action / money-positive-but-neutral** color: `--ph-blue-600` for buttons, links, "미리 받는 돈" totals, and the card channel.
  - **Status:** mint/`--ph-success` for net profit & 이체완료/매입완료; coral `--ph-danger` for fees, 미정산, negatives; orange `--ph-warning` & amber for delivery fees and 예정일 경과.
  - **Channel colors** for sales figures: 카드 = amber `--ch-card`, 배달앱 = green `--ch-delivery`.
  - **Neutrals are a cool blue-gray family.** The dominant muted label is **`--ph-slate #7E8299`**, not a warm gray. Page bg is `--ph-gray-50`.
  - **Admin shell** is a near-black forest-navy rail (`--ph-side-bg #0F1413`) with the green accent on the active nav item.
- **Type.** **Noto Sans KR** for all UI text; **Inter** (tabular, lining) for every numeral and money column via the `.ph-num` helper. Scale tops out around 32px display / big money; body 16, table cells 14, captions 13, badge/micro 12. Headings are 700; black 900 is reserved for the logo badge. Tight `-0.02em` tracking on big numbers.
- **Spacing & layout.** 4px grid. Mobile app is a single centered column (max ~440px) on a near-white bg with a white sticky header. Admin is a 264px fixed sidebar + fluid content (max ~1320px). Min 44px tap targets.
- **Corner radii.** Soft and generous: 6 chips, 8 badges/chips, 10 inputs, 12 buttons & small cards, **16 cards**, **20 large panels / calendars / detail cards**, 999 pills.
- **Cards & elevation.** White surfaces, hairline border *or* a **soft low shadow** (`--sh-card` = barely-there green-tinted double shadow). The app's big cards use a slightly stronger `0 1px 16px rgba(0,0,0,.10)`. Tinted summary cards (success/danger/blue/green) drop the shadow and use a flat tint fill. **No heavy drop shadows, no glows.**
- **Borders & dividers.** 1px hairlines in `--ph-gray-150/200`; list rows are separated by hairlines, not cards. Emphasis borders/inputs are 1.5px. Indented sub-rows use a 1.5px left rule.
- **Backgrounds.** Flat color only — **no gradients, no photographic hero imagery, no patterns or textures.** The single "imagery" is the merchant's own store thumbnail/avatar and bank logos.
- **Motion.** Restrained. `--ease cubic-bezier(.2,.8,.2,1)`, `--dur 180ms` / `--dur-fast 120ms`. Buttons lift 1px and deepen on hover; rows get a `--ph-gray-50` wash on hover; collapsibles rotate a chevron. **No bounce, no parallax, no decorative loops.**
- **States.** Hover = deeper fill (buttons) or faint gray wash (rows/nav). Disabled = gray fill + muted text. Focus = 3px blue ring (`--sh-focus`). Selected calendar day = bright-green circle with forest number.
- **Transparency/blur:** essentially unused — surfaces are opaque. The only alpha is in shadows and a couple of muted-label overlays.

---

## ICONOGRAPHY

- **Custom inline line icons, not an icon library.** Icons are hand-set `<svg viewBox="0 0 24 24">` single `<path>`s with `stroke="currentColor"`, **`stroke-width` 1.7–1.9**, `round` caps & joins, **no fills**. They inherit text color. The admin sidebar defines its full set inline (home / store / chart / chat / settle / box / flask / wallet / users / doc / clock / terminal / refresh); the app uses chevrons and a back arrow the same way.
- **If you need an icon that isn't here,** draw a 24×24 single-stroke path at weight ~1.8 to match — or substitute **Lucide** (same geometric, 1.5–2px stroke, round-cap style) and keep `currentColor`. Flag any substitution.
- **No emoji.** Earlier prototype used a 🏦 glyph for the bank row; this system replaces it with the real `assets/bank-hana.png` logo. Don't introduce emoji.
- **Currency/units are typography, not icons** — the trailing `원` and `₩` prefixes are text.
- **Logo:** the `Payhug` wordmark is text (`Pay` ink + `hug` green-700; inverts to white + bright-green on forest). The app/admin badge is a green rounded square with a black-900 `P`. No raster logo file ships — it's reproducible in CSS (see `guidelines/brand-logo.card.html`).

### Assets (`assets/`)
- `bank-hana.png` — 하나은행 logo (계좌 입금 내역 row).
- `avatar-default.png` — default user/merchant avatar.
- `store-thumb.png` — sample store thumbnail.

---

## INDEX

**Foundations**
- `styles.css` — the single entry point consumers link (imports only).
- `tokens/colors.css` · `tokens/fonts.css` · `tokens/typography.css` · `tokens/spacing.css` — CSS custom properties (base + semantic aliases) + the Google-Fonts webfont import. 119 tokens.
- `guidelines/*.card.html` — foundation specimen cards (Colors, Type, Spacing, Brand groups in the Design System tab).

**Components** (`components/<group>/<Name>.{jsx,d.ts,prompt.md}` — `window.<Namespace>.<Name>`)
- **core:** `Button` · `Badge` · `Avatar` · `Card` · `StatTile`
- **data:** `SalesBreakdown` · `SalesCalendar`
- **feedback:** `AlertBanner`
- **forms:** `Input` · `Select` · `Checkbox`
- **navigation:** `Tabs` · `SegmentedTabs` · `SidebarNav`

**UI kits** (`ui_kits/<product>/index.html`)
- `payhug_app/` — the merchant mobile/web app: 미리 받는 돈, 매출/비용/수익 달력, 카드·배달앱 정산 상세, 미매입·선정산 제외액·예상 차액·계좌 입금 내역. Interactive (push/pop navigation). `PayhugApp.jsx` + `appData.js`.
- `admin_settlement/` — the desktop settlement admin: dark sidebar, 미정산 누락 추적 alert, gaccount summary + merchant & per-transaction tables. `AdminApp.jsx` + `data.js`.
- `cashnote/` — the **CashNote-integrated** merchant app: PayHug app base + a 매출관리 장부 layer (매출관리 상세, 총 비용, 수수료 상세, 총 수익) with period selection. Single self-contained `index.html` (inlined data + app), mounts `window.CNApp`.

**Generated (do not edit):** `_ds_bundle.js`, `_ds_manifest.json`, `_adherence.oxlintrc.json`.

**`SKILL.md`** — makes this folder usable as an Agent Skill in Claude Code.

---

## CAVEATS / SUBSTITUTIONS

- **Webfonts via Google Fonts `@import`** (Noto Sans KR + Inter), matching the original prototype — no font binaries ship in-repo, so the compiler lists 0 `@font-face` faces (they load at runtime). The Figma also used **Apple SD Gothic Neo**, a macOS system face with no webfont; it's covered by the `-apple-system` fallback in the stack. Provide font files if you want them bundled.
- The numbers and screens are realistic **mock data** (잇잇 / 고짬뽕 merchants, 2026-06 dates), not production figures.
