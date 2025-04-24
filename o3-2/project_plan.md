# 🗺️ Luminaire Aromatherapy — End-to-End Build Plan  
_A step-by-step execution guide with progress checklists for junior developers & project managers._

---

## 0. Overview

**Project Goal**  
Build, deploy, and maintain a modular, secure, and extensible e-commerce platform that embodies the premium, sensorial experience of Luminaire Aromatherapy.  The stack is:

* Apache 2.4   + PHP 8.3 (FPM)  
* Minimal MVC micro-kernel using Laravel 12 components  
* MariaDB 11.7 for persistence  
* Tailwind CSS 3.4 & Alpine.js for the UI  
* Stripe for payments

---

## 1. Execution Road-Map (Gantt-friendly)

| Phase | Duration | Milestones |
|-------|----------|------------|
| 1. Foundations        | Day 1 – Day 4  | Repo, env, Docker, database, Tailwind JIT |
| 2. Core MVC Skeleton  | Day 5 – Day 8  | Routing, layout, home, product show       |
| 3. Commerce Modules   | Day 9 – Day 14 | Cart, Checkout, Stripe, Orders            |
| 4. CMS & Admin        | Day 15 – Day 20| CRUD, dashboard, ACL                      |
| 5. QA & Hardening     | Day 21 – Day 24| Tests, security, performance              |
| 6. Launch & Monitor   | Day 25 – Day 28| CI/CD, blue/green, observability          |

> _Buffer_: 2 days for unforeseen issues.  
> _Total_: **30 calendar days** (≈ 140 dev hours).  

---

## 2. Detailed Step-by-Step Plan

Each step follows the same template:

```
Objective   — what success looks like  
Actions     — concrete coding/ops tasks  
Checklist   — markdown boxes to tick ✅
```

---

### Step 1 · Local Environment & Repo (Day 1)

**Objective**  
Developer has a reproducible local stack (PHP 8.3, Node 18, MariaDB) and the GitHub repo cloned.

**Actions**  
1. Install VS Code + PHP extensions.  
2. Install Docker Desktop / Podman.  
3. `git clone https://github.com/your-org/luminaire.git`  
4. `composer install`   +  `npm install`.  
5. Copy `.env.example` → `.env`.  
6. Launch Dev DB: `docker compose up -d mariadb`.  
7. Verify PHP CLI `php -v` == 8.3.

**Checklist**

- [ ] VS Code & PHP extensions installed  
- [ ] Docker running (ping Docker Desktop)  
- [ ] Repo cloned to `~/Projects/luminaire`  
- [ ] `composer install` passes w/o error  
- [ ] `npm install` completes  
- [ ] `.env` created with local creds  
- [ ] MariaDB container responding on `3306`  

---

### Step 2 · Application Bootstrap (Day 1-2)

**Objective**  
Micro-kernel boots, serves a plain “It works” page at `http://localhost:8000`.

**Actions**  
1. Open `bootstrap/app.php`, ensure service providers registered.  
2. Wire `public/index.php` to load the kernel.  
3. Create first route in `routes/web.php` → lambda returns “Hello Luminaire”.  
4. Run `php -S localhost:8000 -t public` (or Sail).  
5. Confirm browser response.

**Checklist**

- [ ] `bootstrap/app.php` loads w/o fatal errors  
- [ ] `routes/web.php` first route added  
- [ ] Server responds at `/` with handshake string  

---

### Step 3 · Database & Migrations (Day 2)

**Objective**  
Core schema migrated; seed demo data.

**Actions**  
1. Implement `/includes/db.php` (PDO).  
2. Write migration classes in `/database/migrations` for `users`, `products`, `orders`, etc.  
3. Build simple CLI `artisan-lite` to run migrations.  
4. Seed 5 categories, 8 products, 1 admin user (`password: secret`).  

**Checklist**

- [ ] `php artisan-lite migrate` creates all tables  
- [ ] `php artisan-lite db:seed` inserts demo rows  
- [ ] `SELECT COUNT(*) FROM products` returns 8  

---

### Step 4 · Tailwind & Asset Pipeline (Day 2)

**Objective**  
Tailwind JIT compiles, Vite Hot-Reload works.

**Actions**  
1. Edit `resources/tailwind.config.js` (paths to `/views/**/*.php`).  
2. Add scripts to `package.json`:  
   ```json
   "dev": "vite",
   "build": "vite build"
   ```  
3. Run `npm run dev` – ensure console shows “tailwindcss – watching…”.  
4. Create `/views/test.php` with `<div class="text-accent">` style check.

**Checklist**

- [ ] `npm run dev` shows Tailwind watcher  
- [ ] Visiting `/test` renders accent-coloured text  
- [ ] `npm run build` outputs hashed CSS/JS to `public/assets`  

---

### Step 5 · Global Layout & Navigation (Day 3)

**Objective**  
Glass-morphism header, footer, master layout wired.

**Actions**  
1. Build `views/layout/master.php` with HTML5 head, meta, Vite assets.  
2. Extract header & footer partials.  
3. Populate nav links (`Shop`, `Rituals`, `Journal`, `About`).  
4. Copy dark/light theme toggle JS.  
5. Verify focus rings & responsive menu.

**Checklist**

- [ ] `master.php` exists & references `@yield('content')` slots  
- [ ] Header fixed, translucent; footer sticky at page end  
- [ ] Light/dark toggle persists in `localStorage`  

---

### Step 6 · Home / Landing Page (Day 3-4)

**Objective**  
Hero video + “Bestsellers” section renders using live product data.

**Actions**  
1. Convert HTML landing prototype → `/views/home.php`.  
2. Query 4 top-selling products via `Product::limit(4)->get()`.  
3. Loop in Blade-less syntax `foreach ($products as $item)…`.  
4. Implement IntersectionObserver reveal in `/public/assets/js/app.js`.  
5. QA on mobile viewport.

**Checklist**

- [ ] Hero shows slow-zoom video + particles  
- [ ] Products load dynamically from DB  
- [ ] Scroll reveal triggers once per section  
- [ ] Mobile < 400 px layout stacks correctly  

---

### Step 7 · Product Detail Page (Day 4-5)

**Objective**  
Dynamic route `/product/{slug}` with images, price, add-to-cart.

**Actions**  
1. Add `ProductController@show` + route.  
2. Build `views/product/show.php`: gallery, description, reviews.  
3. Implement `Review` relationship; show average rating.  
4. “Add to cart” form posts to `/cart/add` (AJAX later).  

**Checklist**

- [ ] Slug resolves (404 on invalid)  
- [ ] Breadcrumb `Home › Category › Product` appears  
- [ ] Rating stars render from DB  
- [ ] Add-to-cart button posts correct payload  

---

### Step 8 · Cart Module (Day 5-6)

**Objective**  
Session-based cart (or DB for logged-in users) with mini-bag counter.

**Actions**  
1. Create `CartService` (add, remove, totals).  
2. AJAX endpoint `/cart/add`, `/cart/remove`.  
3. Mini cart drawer component (Alpine.js) updates count.  
4. `/cart` page lists items with subtotal.

**Checklist**

- [ ] Add 2 items → mini badge increments  
- [ ] Removing item updates totals instantly  
- [ ] Refreshing page preserves cart state  
- [ ] Empty cart CTA encourages browsing  

---

### Step 9 · Authentication (Day 6)

**Objective**  
Login, registration, hashed passwords, CSRF.

**Actions**  
1. Database `users` migration (bcrypt).  
2. Build `AccountController` (loginForm, login, register, logout).  
3. CSRF middleware; token blade-less helper.  
4. Protect `/checkout`, `/account/dashboard` with `AuthMiddleware`.

**Checklist**

- [ ] Registration inserts user row  
- [ ] Password stored with `password_hash`  
- [ ] Login sets secure, HTTP-only cookie  
- [ ] CSRF mismatch returns 419  

---

### Step 10 · Checkout Form (Day 7-8)

**Objective**  
Collect address, review order, create Stripe PaymentIntent.

**Actions**  
1. `CheckoutController@form` shows billing & shipping sections.  
2. Use `CheckoutValidator` for server rules.  
3. Create PaymentIntent via Stripe PHP SDK.  
4. Render Stripe Elements card field.  
5. On success, insert `orders` + `order_items`, clear cart.

**Checklist**

- [ ] Address validation fails gracefully  
- [ ] PaymentIntent secret passed to JS  
- [ ] Order row created with correct totals  
- [ ] Confirmation e-mail queued  

---

### Step 11 · Webhooks (Day 8)

**Objective**  
Stripe webhook endpoint verifies signatures, updates order status.

**Actions**  
1. Expose `/webhook/stripe` route (no CSRF).  
2. Verify using `STRIPE_WEBHOOK_SECRET`.  
3. Handle `payment_intent.succeeded` → mark `orders.status = paid`.  
4. Log payloads to `storage/logs/stripe.log` for debugging.

**Checklist**

- [ ] CLI test: `stripe trigger payment_intent.succeeded` passes  
- [ ] Order status flips to “paid” in DB  
- [ ] Invalid signature returns 400  

---

### Step 12 · ACL Middleware (Day 9)

**Objective**  
Role-based access (`admin`, `customer`).

**Actions**  
1. Add `role` column to `users` migration.  
2. `AdminMiddleware` redirects non-admins.  
3. Seed default admin user.  

**Checklist**

- [ ] Accessing `/admin/dashboard` as guest → /login  
- [ ] Logged-in customer still blocked  
- [ ] Admin user sees dashboard  

---

### Step 13 · Admin CRUD (Day 9-10)

**Objective**  
Admins can create/modify products & categories.

**Actions**  
1. Controllers inside `App\Controllers\Admin`.  
2. Use Tailwind tables & modals for CRUD UI.  
3. File upload handling (`thumbnail`) with MIME whitelist.  
4. Slug auto-generation via JS (`slugify`).  
5. Flash messages for success / error.

**Checklist**

- [ ] Create new product appears on storefront  
- [ ] Editing price updates live  
- [ ] Deleting product cascades to `order_items` soft‐delete  
- [ ] Thumbnail stored under `storage/app/products`  

---

### Step 14 · CMS Blocks (Day 10-11)

**Objective**  
Marketing team can edit hero copy, story paragraphs, testimonials.

**Actions**  
1. `pages` table (`key`, `json_blob`).  
2. Admin WYSIWYG (TipTap) saves JSON; front-end decodes.  
3. Allow images via `content_images/` directory.  

**Checklist**

- [ ] Editing hero headline reflects immediately  
- [ ] JSON schema validated on save  
- [ ] Uploaded images sanitized  

---

### Step 15 · API (Day 11)

**Objective**  
Public read-only REST endpoints.

**Actions**  
1. Sanctum install; token issuance route `/api/token`.  
2. `routes/api.php` → `ProductApiController@index/show`.  
3. Rate limit middleware (`throttle:120,1`).  

**Checklist**

- [ ] GET `/api/v1/products` returns JSON list  
- [ ] GET `/api/v1/products/{id}` returns full object  
- [ ] Requests without token still succeed (read-only)  
- [ ] Abuse beyond 120/min returns 429  

---

### Step 16 · Automated Tests (Day 12-13)

**Objective**  
80 % unit & feature coverage.

**Actions**  
1. Install PestPHP + PHPUnit.  
2. Unit tests for `CartService`, `Product` price accessor.  
3. Feature test: add → checkout → order paid (mock Stripe).  
4. JS unit via Vitest (`cartDrawer.spec.js`).  

**Checklist**

- [ ] `./vendor/bin/pest` green  
- [ ] Coverage report > 80 %  
- [ ] Failing test breaks GitHub Action  

---

### Step 17 · Security Audit (Day 14)

**Objective**  
Pass OWASP top 10 checklist.

**Actions**  
1. Run `composer audit` + `npm audit`.  
2. Pen-test with ZAP baseline scan.  
3. Ensure CSP headers via Apache conf.  
4. Verify HTTPS, HSTS set on staging.

**Checklist**

- [ ] No critical CVEs from audit tools  
- [ ] ZAP reports < 3 medium issues, 0 high  
- [ ] CSP header present & correct  
- [ ] Cookies set `Secure; HttpOnly; SameSite=Lax`  

---

### Step 18 · Performance Tune (Day 15)

**Objective**  
Lighthouse ≥ 90 on mobile.

**Actions**  
1. Add `loading="lazy"` to images; preconnect fonts.  
2. Enable Brotli & `Expires` headers.  
3. Configure OPcache 256 MB in `docker/php.ini`.  
4. Defer non-critical JS.

**Checklist**

- [ ] Mobile LCP < 2.5 s  
- [ ] CLS < 0.1  
- [ ] Lighthouse Performance ≥ 90  

---

### Step 19 · CI/CD & Staging Deploy (Day 16-18)

**Objective**  
Automatic deployment to staging URL on push to `main`.

**Actions**  
1. Write GitHub Action workflow:  
   * Checkout → Composer → Pest → Vitest → Vite build.  
   * Build multi-stage Docker image.  
   * Push to GH Container Registry.  
2. DigitalOcean droplet + `docker compose up -d`.  
3. Add staging secrets (`.env.staging`).  

**Checklist**

- [ ] `main` push triggers pipeline  
- [ ] Staging site reachable at `staging.luminaire.com`  
- [ ] Rollback command documented  

---

### Step 20 · Production Cut-over (Day 25-28)

**Objective**  
Zero-downtime launch with blue/green.

**Actions**  
1. Duplicate prod DB from staging (excluding test orders).  
2. Provision load balancer, two app pools (blue & green).  
3. Deploy `green`, run migration.  
4. Smoke tests (`curl /health`).  
5. Switch LB DNS to `green`, monitor 30 min, delete `blue`.  

**Checklist**

- [ ] DB migrated with no errors  
- [ ] Health endpoint 200 for 5 min before switch  
- [ ] Error rate < 1 % post-switch  
- [ ] Search console & analytics updated  

---

## 3. Master Checklist (Manager View)

| Phase | Item | Owner | ETA | ✅ |
|-------|------|-------|-----|----|
| Foundations | Local env ready | Dev | D1 | [ ] |
| Foundations | DB migrated & seeded | Dev | D2 | [ ] |
| Core MVC | Layout & Home page | Dev | D4 | [ ] |
| Core MVC | Product detail page | Dev | D5 | [ ] |
| Commerce | Cart module | Dev | D6 | [ ] |
| Commerce | Auth & checkout | Dev | D8 | [ ] |
| Commerce | Stripe webhook | Dev | D8 | [ ] |
| Admin | ACL middleware | Dev | D9 | [ ] |
| Admin | Product CRUD | Dev | D10 | [ ] |
| CMS | Hero/story blocks | Dev | D11 | [ ] |
| API | v1 endpoints | Dev | D11 | [ ] |
| QA | 80 % test coverage | QA | D13 | [ ] |
| Security | OWASP pass | Sec | D14 | [ ] |
| Performance | Lighthouse 90+ | Dev | D15 | [ ] |
| CI/CD | GitHub Action & Staging | DevOps | D18 | [ ] |
| Launch | Blue/green production | DevOps | D28 | [ ] |

---

## 4. Conclusion

Following this granular plan ensures that a junior developer can deliver **Luminaire Aromatherapy** from repository to revenue in a controlled, trackable manner.  Each step contains a measurable definition of done and built-in checkpoints for both the developer and project manager.  

✅ **Rule of thumb**: Don’t move to the next step until every box in the current checklist is ticked.

> _“Small steps, deliberate cadence, premium results.”_

Happy coding & may your console stay green!  
_Luminaire Core Team_
