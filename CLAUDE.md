# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BNI Vietnam Member Shop — a static HTML/CSS website for selling BNI membership packages. No build tools, no JavaScript framework, no package manager. Just open HTML files directly in a browser.

## Architecture

**Page types:**
- **Shop page** (`index.html`): Products split into two labelled groups — "Gói Thành Viên Mới" (2 cards, `.member-shop-2`) and "Gói Gia Hạn Thành Viên" (4 cards, `.member-shop` 4-col). Uses `css/shared.css` + `css/shop.css`.
- **Detail pages** (`goi-*.html`): Individual product pages with image, pricing, tabbed content (benefits/description/policy), and related products. Uses `css/shared.css` + `css/detail.css`.

**CSS structure:**
- `css/shared.css` — Reset, base typography (Roboto), header, footer, `.product-card` base styles, Baokim payment-button overrides (`.bk-btn-*`), tab-content typography (`.desc-text`, `.policy-group`, `.features-list-spaced`), and shared responsive breakpoints.
- `css/shop.css` — Shop grid layout (`.member-shop` 4-col grid), two-group layout (`.shop-group`, `.group-title`, `.member-shop-2` — max-width 570px = exactly 2 cols + 1 gap of the 4-col grid so cards stay the same width), product card sizing for shop, `.btn-cart`.
- `css/detail.css` — Product detail layout, price block, tabs system, benefits grid, policy/refund table, related products section (`.member-shop` 3-col grid), `.btn-cart-sm`.

**Key conventions:**
- Brand color: `#CF2030` (BNI red), used consistently across headers, buttons, accents.
- Language: Vietnamese (`lang="vi"`). All content is in Vietnamese.
- Prices include hidden `<input type="hidden" class="price-raw">` fields with `data-product-id` attributes (e.g., `member-new-1year`, `member-renew-2year`) for potential payment integration.
- Product images are hosted externally on `bni.vn`.
- Tab switching on detail pages uses inline `onclick="switchTab()"` with vanilla JS.
- No inline CSS: HTML files carry no `<style>` blocks and no `style=""` attributes — everything lives in `css/`.
- Header and footer markup is duplicated across all HTML files (no templating).
- Logo in header is an `<a href="index.html" class="logo">` (clickable, links to home).
- Nav "Sản phẩm" links to `index.html`.
- Breadcrumbs (detail pages only): 2 parts — `<a href="index.html">Trang chủ</a> / <span>Tên trang</span>`.

## Products

Prices below are effective 01/09/2026 (new-member packages rose by 108,000 from 17,820,000 / 30,545,940).

| ID | Page | Code | Price (VND) |
|---|---|---|---|
| `member-new-1year` | `goi-thanh-vien-moi-01-nam.html` | BNI-New 01 | 17,928,000 |
| `member-new-2year` | `goi-thanh-vien-moi-02-nam.html` | BNI-New 02 | 30,653,940 |
| `member-renew-1year` | `goi-gia-han-01-nam.html` | BNI-Renew-01 | 15,660,000 |
| `member-renew-2year` | `goi-gia-han-02-nam.html` | BNI-Renew-02 | 28,385,940 |
| `member-renew-overdue-1year` | `goi-gia-han-01-nam-tre-han.html` | BNI-Renew-OD-01 | 16,200,000 |
| `member-renew-overdue-2year` | `goi-gia-han-02-nam-tre-han.html` | BNI-Renew-OD-02 | 28,925,940 |

Overdue ("trễ hạn") packages = the matching renewal price + a flat **540,000** late fee. Keep that arithmetic intact — both pages state the breakdown to the customer.

Each detail page's "Các Gói Thành Viên Khác" grid is fixed at 3 cards. With 6 products it cannot list every other package, so each page links its nearest neighbours: new-member pages show the two renewals, renewal pages show the two overdue packages, overdue pages show the two standard renewals.

## Responsive Breakpoints

- `900px`: Nav collapses to mobile menu; detail grid switches to 2-col related products
- `768px`: Product detail splits to single column; footer to 2-col
- `600px`: Shop/related grids to single column; benefits grid to single column
- `480px`: Footer to single column
