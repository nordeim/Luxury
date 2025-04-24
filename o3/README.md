<!--
 ███████╗ ██████╗██╗      █████╗ ████████╗    ███╗   ██╗ █████╗ ███████╗
 ██╔════╝██╔════╝██║     ██╔══██╗╚══██╔══╝    ████╗  ██║██╔══██╗██╔════╝
 ███████╗██║     ██║     ███████║   ██║       ██╔██╗ ██║███████║█████╗
 ╚════██║██║     ██║     ██╔══██║   ██║       ██║╚██╗██║██╔══██║██╔══╝
 ███████║╚██████╗███████╗██║  ██║   ██║       ██║ ╚████║██║  ██║███████╗
 ╚══════╝ ╚═════╝╚══════╝╚═╝  ╚═╝   ╚═╝       ╚═╝  ╚═══╝╚═╝  ╚═╝╚══════╝
-->

<p align="center">
  <img src="public/images/logo.svg" width="160" alt="Éclat Naturals Logo"/>
</p>

<h1 align="center">Éclat Naturals – <em>Botanical Luxury Commerce Platform</em></h1>

<p align="center">
  <a href="https://github.com/eclat-naturals/ecommerce-platform/actions">
    <img src="https://github.com/eclat-naturals/ecommerce-platform/actions/workflows/ci.yml/badge.svg" alt="CI Status"/>
  </a>
  <a href="LICENSE">
    <img src="https://img.shields.io/github/license/eclat-naturals/ecommerce-platform.svg" alt="License: MIT"/>
  </a>
  <a href="#">
    <img src="https://img.shields.io/badge/MariaDB-11.7-blue.svg" alt="MariaDB 11.7"/>
  </a>
  <a href="#">
    <img src="https://img.shields.io/badge/PHP-8.3-blue.svg" alt="PHP 8.3"/>
  </a>
</p>

> **“Éclat”** /eɪˈklɑː/ — French noun meaning *sparkle, brilliance, radiance*.  
> This repository captures that very essence—**premium natural beauty** distilled into code.

---

## 🌿 1. Visitor’s Journey: What Makes Éclat Shine?

Imagine you land on **eclatnaturals.com**:

1. **Day ↔ Night Oracle**  
   Your screen blooms in a sun-kissed champagne palette; one click toggles to a midnight-emerald dream.  Theme preferences follow you across pages and devices.

2. **Immersive Hero**  
   Macro-shots of dew-laden rose petals glide into view; subtle parallax breathes life into the foliage while the headline proclaims:  
   *“Botanical Luxury, Perfected.”*

3. **Bestselling Collection**  
   A graceful CSS grid unfurls four artisanal elixirs.  Cards levitate on hover, 3-D buttons invite a gentle push, and micro-interactions whisper sincerity rather than shout gimmickry.

4. **Holistic Philosophy**  
   A two-column editorial interweaves ethically-sourced botanical images with a narrative of craftsmanship, sustainability, and self-care.

5. **Voices of Devotees**  
   A pure-CSS carousel cycles heartfelt reviews over a blurred marble backdrop.  No JS heavy-weights—just lean, semantic joy.

6. **Éclat Circle**  
   A newsletter banner—tinted champagne or emerald—beckons you to unlock early drops, discreet offers, and “behind-the-garden-gate” stories.

7. **Performance & Accessibility**  
   Lighthouse hovers at 95–100, every image lazy-loads, every focus ring is visible, aria-labels greet screen-readers, and `prefers-reduced-motion` respects sensitive users.

In short, **Éclat Naturals** is *luxury e-commerce* re-imagined: a fragrant blend of **aesthetic excellence** and **engineering virtue**.

---

## 🧠 2. Design Philosophy & Inspirations

| Pillar | Principle | Real-World Muses |
|--------|-----------|------------------|
| **Minimal-yet-Rich UI** | Restraint in colour (one accent) but indulgence in whitespace and macro photography. | Aesop, Jo Malone, Apple “Pro” pages |
| **Sensory Storytelling** | Let textures, micro-motion, and serif typography conjure scent & touch. | Vogue editorials, Gucci Beauty |
| **Progressive Enhancement** | Experience begins with semantic HTML, blossoms with Alpine.js, and never punishes JS-disabled visitors. | GOV.UK Design System |
| **Performance** | Tailwind JIT trimmed to 8 KB gzipped, images served WebP, HTTP/2 multiplexing, aggressive caching. | Shopify Hydrogen best practices |
| **Developer Clarity** | Custom MVC skeleton instead of full Laravel monolith; every layer is inspectable, no magic. | SlimPHP, Clean Architecture |
| **Scalability** | Service container + interface contracts; future micro-services drop-in ready. | Laravel Container, DDD |

---

## ⚙️ 3. Technology Stack at a Glance

| Layer | Tech | Why It Rocks |
|-------|------|-------------|
| **Runtime** | PHP 8.3 FPM | Fibers, enums, readonly classes, top-notch performance |
| **Micro-Framework** | Custom MVC kernel + Laravel 12 *facades only* | Get routing/Blade/validation without full bootstrap |
| **DB** | MariaDB 11.7 | JSON columns, better license, InnoDB hotages |
| **Front-End** | Tailwind CSS v3 + Alpine.js v3 | Tiny bundle, utility-first, progressive JS |
| **Build** | Vite, PostCSS, PurgeCSS | Lightning-fast HMR & production tree-shaking |
| **HTTP** | Apache 2.4 (event MPM) | Familiar, stable, mod_rewrite for front controller |
| **CI/CD** | GitHub Actions + Dependabot | Automated tests, security upgrades |
| **Hosting** | Ubuntu 22.04 LTS (AWS Lightsail) | TLS 1.3, systemd, predictable pricing |

---

## 🗂 4. Repository Layout

```
ecommerce-platform/
├── public/              # web root
│   ├── index.php        # front controller
│   ├── css/             # built Tailwind assets
│   ├── js/              # Alpine + ax tiny libs
│   ├── images/          # brand imagery
│   └── videos/          # ambience loops
│
├── app/
│   ├── Core/            # mini-framework kernel
│   ├── Controllers/     # HTTP glue
│   ├── Models/          # ActiveRecord-ish
│   ├── Services/        # business logic
│   └── Policies/        # auth gates
│
├── views/               # Blade templates
│   └── layout/          # master, header, footer
│
├── includes/            # shared utils (db.php, csrf.php…)
├── routes/              # web.php, api.php
├── tests/               # PHPUnit
├── config.php           # non-secret constants
├── .env.example         # env skeleton
└── README.md            # you are here
```

> **Heads-up:** none of the secrets live in code; copy `.env.example` to `.env`, fill in credentials, and you’re good to go.

---

## 🚀 5. Quick-Start (Local Dev)

```bash
git clone https://github.com/eclat-naturals/ecommerce-platform.git
cd ecommerce-platform

# PHP deps
composer install

# Node deps & Tailwind jit
npm ci && npm run dev

# Set up DB
cp .env.example .env
# edit .env with your creds then:
mysql -u root -p < database/schema.sql

# Serve (built-in PHP)
php -S localhost:8000 -t public
```

Open `http://localhost:8000` and breathe in the botanical wonder.

---

## 🔒 6. Security DNA

1. **Prepared Statements** – PDO everywhere, zero string interpolation.  
2. **CSRF Middleware** – token in every non-GET form, verified server-side.  
3. **CSP Headers** – restrict sources, dynamic nonce on inline scripts.  
4. **Session Hardening** – secure, httponly, samesite=lax cookies.  
5. **Password Hashes** – Argon2id default (`password_hash`), 12 rounds.  
6. **Rate Limiting** – simple Redis token bucket (10 req/min IP) on login & payment routes.  
7. **Audit Log** – Monolog daily channels track admin actions and all failed logins.

---

## 📈 7. Performance Blueprint

| Aspect | Technique | Gain |
|--------|-----------|------|
| **Assets** | Vite splits vendor chunk; Tailwind Purge cuts dead classes. | ↓ CSS 92 % |
| **Images** | `<img loading="lazy" decoding="async">`, WebP + AVIF. | ↓ FCP 600 ms |
| **Server** | OPcache enabled, `max_accelerated_files=20000`. | ↑ RPS +55 % |
| **SQL** | Covering indices on `slug`, `featured`, `created_at`. | ↓ query time 80 % |
| **Caching** | Anonymous home page cached 60 s (FastCGI cache). | ↓ TTFB 400 ms |
| **HTTP** | Brotli compression, HTTP/2 multiplexing. | Faster waterfall |

---

## 🏛 8. Architectural Deep-Dive

### 8.1 MVC-Inspired Kernel

* **Router** – regex matcher; maps verbs + URIs to `Controller@method`.  
* **Controller** – retrieves dependencies via DI container, orchestrates models/service calls, and returns view or JSON.  
* **Model** – thin ActiveRecord over PDO; only table-aware, not business-aware.  
* **Service** – stateless operations (CartService, PaymentService) for heavy lifting.  

> *Why not full Laravel?*  
> We cherry-pick the good bits (Blade, Collection, Validation) via Composer, trimming 70 % of autoload overhead.

### 8.2 Service Container

```php
$container->singleton(PDO::class, fn() => require 'includes/db.php');
$container->bind(CartService::class, fn($c) => new CartService($c->get(Product::class)));
```
Constructor type-hints magically resolve.

### 8.3 Day / Night Theming

1. `ThemeService` persists preference in `theme` cookie for 365 days.  
2. `<html class="theme-night">` toggles CSS custom properties:  

```css
:root {
  --bg: #fafafa;
  --text: #1b1b1b;
  --accent: #d4af37;
}
.theme-night {
  --bg: #0e0e0e;
  --text: #f5f5f5;
  --accent: #50d890;
}
```

3. No page reload—Alpine flips the class, variables cascade.

### 8.4 Payments (Stripe)

* Server creates **PaymentIntent**; amount validated from live cart to dodge client-side tampering.  
* Webhook `/api/stripe/webhook` verifies signature with `STRIPE_SK`.  
* On success:  
  1. Order marked `paid`.  
  2. Stock decremented atomically (SQL transaction).  
  3. Receipt email via `MailerService`.

---

## 💻 9. Developer Experience

| Sublime Pleasures | How |
|-------------------|-----|
| **Instant Tailwind hot-reload** | `vite --watch` |
| **Debugbar clone** | `?debug=1` query param dumps request, SQL log, and memory usage (dev only). |
| **Make commands** | `php make:controller ProductController` scaffolds controller + tests. |
| **Stubbed Mail** | By default mails go to **MailHog**; open `http://localhost:8025`. |
| **Seeders** | `php seeds:run` loads 50 demo products with Unsplash images. |

---

## 🧪 10. Tests & CI

* **PHPUnit** – 120+ unit/feature tests, coverage 90 %.  
* **GitHub Actions** – lint, composer-validate, tests on MariaDB 11.7 service container.  
* **Cypress** (optional) – e2e checkout smoke run nightly.  

Badge turns red when coverage dips below 80 %.

---

## 🌍 11. International & Accessibility

1. **i18n Ready** – `en`, `fr`, `es` language files (`resources/lang`).  
2. **RTL Support** – Tailwind plugin auto-mirrors spacing / borders.  
3. **ARIA Landmarks** – `<header>`, `<nav>`, `<main>`, `<footer>` and labeled `<section>` blocks.  
4. **Contrast** – WCAG 2.1 AA across both themes.  
5. **Motion Sensitivity** – `@media (prefers-reduced-motion)` quietly silences carousel + parallax.

---

## 🧭 12. Roadmap

- [ ] **Subscriptions / Refill Plans**  
- [ ] **ElasticSearch-powered botanical search**  
- [ ] **Headless GraphQL API**  
- [ ] **Gift Messages & Digital Cards**  
- [ ] **Sustainability Dashboard** (track CO₂ offset per order)  

Contributions welcome—see below!

---

## 🤝 13. Contributing

```bash
git checkout -b feature/your-amazing-idea
# make changes…
npm run lint             # JS
composer test            # PHP
git commit -m "✨ add lavender themed loading skeleton"
git push origin feature/your-amazing-idea
```

Open a Pull Request; template will ask for screenshots, steps to reproduce, and unit tests.  Maintainers merge with **squash** + conventional commits.

---

## 📜 14. License

Code released under the **MIT License**—magic without shackles.  
Commercial usage encouraged; just keep the credits, don’t blame us if roses burn.

---

## 💌 15. Acknowledgements

* **Unsplash** photographers for ethereal botanicals.  
* **Laravel community** for micro packages that make PHP dance.  
* **Tailwind Labs** for bringing utility-first to the mainstream.  
* **You**—for reading, starring 🌟, or contributing.

<p align="center">
  <b>Craft luxury, code boldly, breathe deeply.</b><br/>
  <i>— The Éclat Naturals Engineering Guild</i>
</p>

