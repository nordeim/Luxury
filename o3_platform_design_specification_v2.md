# Technical Design Specification  
_A modular, secure & extensible PHP-based e-commerce platform for premium aromatherapy products_

---

## 0. Table of Contents
1. Executive Summary  
2. Business & Product Goals  
3. Non-Functional Requirements  
4. Technology Stack  
5. High-Level Architecture  
6. Detailed Application Layers  
7. File & Directory Layout  
8. Configuration Management (`/config.php`, `/includes/db.php`)  
9. Database Schema & ER Diagram  
10. Routing Map  
11. Controllers  
12. Models  
13. Views, Partials & Tailwind CSS Workflow  
14. Security Hardening  
15. Payment & Third-Party Integrations  
16. Admin Back-Office  
17. Testing Strategy  
18. CI/CD & Deployment  
19. Performance & Observability  
20. Future Extensibility & Road-Map  
21. Appendix (Code Conventions, Composer.json, .env example, helper functions)

Approximate length: **~5,600 words (~34k characters)**

---

## 1. Executive Summary
This design document describes, step-by-step, how to build a production-grade, developer-friendly e-commerce platform dedicated to high-end aromatherapy products. The system embraces a **lean MVC** pattern in **PHP 8.3** but deliberately avoids “black-box” frameworks. Instead, it layers a minimal micro-kernel on top of **Laravel 12 components** (HTTP, Routing, Validation, Eloquent ORM) to gain best-of-class productivity while keeping full code visibility.

Key attributes:

* **Modularity** – clear separation of concerns, Composer autoloading, reusable Blade-less view partials.  
* **Security** – sane defaults (CSP, rate-limiting, prepared statements), OWASP adherence, user roles.  
* **Extensibility** – each module (Catalog, Cart, Checkout, CMS, API) is isolated, versioned and testable.  
* **Developer Experience** – Tailwind JIT, Hot Module Reload (Vite), uniform coding standards (PSR-12).  

By following this guide, a junior developer can clone the repo, run `composer install && npm install && npm run dev`, create a `.env` file, migrate the DB, seed fixtures, and have a running store in < 15 minutes.

---

## 2. Business & Product Goals

| Goal | Detail |
|------|--------|
| Premium digital flagship | Mirror the luxury feel of the landing page with buttery UX, micro-animations and immaculate typography. |
| End-to-end commerce | Browsing → Cart → Checkout → Payments → Fulfillment → Review. |
| Omnichannel ready | Public REST/JSON API for mobile apps & headless storefronts. |
| Content & Storytelling | CMS blocks (ingredients, provenance, blog) editable by non-tech stakeholders. |
| Global growth | Multi-currency, i18n, GDPR & CCPA compliance. |
| Maintainable cost-center | Minimal third-party SaaS, infra on vanilla **LAMP** or containerized **Docker**. |

---

## 3. Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| Performance | Sub-2 s TTFB, LCP <= 2.5 s on 3G, score 90+ on Lighthouse. |
| Security | PCI-DSS SAQ A, OWASP Top10 2023, CSP default-src 'self'. |
| Availability | 99.9 % uptime, blue/green deploys, DB backups every 1 h. |
| Scalability | Horizontal scaling behind HAProxy; stateless app servers. |
| Observability | Structured JSON logs, OpenTelemetry traces, Grafana dashboards. |
| Accessibility | WCAG 2.2-AA, keyboard-only, `prefers-reduced-motion`. |
| Legal | Cookie consent, right-to-be-forgotten, age gate (≥ 13 yrs). |

---

## 4. Technology Stack

| Layer | Technology | Reasoning |
|-------|------------|-----------|
| Web Server | Apache 2.4 (prefork + mpm_event) | Proven, easy .htaccess overrides, mod_security. |
| Runtime | PHP 8.3 FPM | Typed properties, enums, Fibers, faster JIT. |
| Micro-Kernel | Laravel 12 Components (HTTP Kernel, Routing, Validation, Eloquent) | Keep familiar APIs but tiny footprint. |
| DB | MariaDB 11.7 | MySQL-compatible, better clustering options. |
| Front-end | Tailwind CSS v3 + Alpine.js | Utility-first styling & sprinkle-JS. |
| Build | Vite 5 (ESBuild) | Instant reload, tree-shaking, TS support. |
| Package Mgr | Composer 2, NPM 10 | Dependency hygiene. |
| Auth | Laravel Sanctum + CSRF tokens | Simple cookie-based API auth. |
| Payments | Stripe Elements (PCI SAQ A) | Reduces card-holder scope. |
| Container | Docker Desktop or Podman (dev) | Reproducible local stacks. |

---

## 5. High-Level Architecture
```
 ┌────────────┐    HTTPS     ┌──────────────┐
 │   Client   │◀────────────▶│   Apache 2.4 │
 └────────────┘              └──────┬───────┘
                                    │ FPM
                       ┌────────────▼────────────┐
                       │   PHP 8.3 Application   │
                       │  (Custom MVC Kernel)    │
                       └────────────┬────────────┘
                                    │ PDO
                       ┌────────────▼────────────┐
                       │     MariaDB 11.7        │
                       └─────────────────────────┘
```

* _Static assets_ are served by Apache with far-future cache headers.  
* _Dynamic requests_ (`/shop`, `/cart`) are routed to `public/index.php` which boots the micro-kernel.  
* _Eloquent_ models communicate with MariaDB via PDO; queries logged with Telescope-lite.  
* Optional _Redis_ cache (for sessions, config, views) can be added later without impacting code.

---

## 6. Detailed Application Layers

### 6.1 Kernel
A thin bootstrap file (`bootstrap/app.php`) registers:

1. Error handler (`Whoops` in dev, custom JSON in prod).  
2. Dependency injection container (Illuminate\Container).  
3. Service providers (Routing, Database, Session, View, Validation).  

Total boot time budget: **< 50 ms**.

### 6.2 Routing
`routes/web.php` – maps URL patterns to **Controller@method**.  
`routes/api.php` – versioned `/api/v1/*` endpoints (JWT/Sanctum).

Example:
```php
Route::get('/product/{slug}', [ProductController::class, 'show'])
     ->name('product.show')
     ->middleware('throttle:60,1');
```

### 6.3 Controllers
Skinny, stateless, dependency-injected via constructor. Delegates:

* Data retrieval – **Model** or **Service**  
* View rendering – **View::make()**  
* Form validation – **Illuminate\Validation\Factory**

### 6.4 Models
Only business logic (OO domain methods), extend `Illuminate\Database\Eloquent\Model`.  
Soft-deletes, factories, observers for auditing.

### 6.5 Views
Vanilla PHP templates in `/views`, no Blade to reduce magic. Use **Tailwind** classes.  
`layout/master.php` contains `<head>` and global header/footer partials.

### 6.6 Services
Reusable components (e.g., CartService, PaymentService) in `/services`.

### 6.7 Helpers
Globally namespaced functions in `/helpers.php` (e.g., `price($cents)` returns formatted currency).

---

## 7. File & Directory Layout

```
lux-aroma/
├─ app/
│  ├─ Controllers/
│  │   ├─ HomeController.php
│  │   ├─ ProductController.php
│  │   ├─ CartController.php
│  │   ├─ CheckoutController.php
│  │   ├─ AccountController.php
│  │   └─ Admin/
│  │       ├─ DashboardController.php
│  │       ├─ ProductAdminController.php
│  │       └─ OrderAdminController.php
│  ├─ Models/
│  │   ├─ Product.php
│  │   ├─ Category.php
│  │   ├─ User.php
│  │   ├─ Order.php
│  │   ├─ OrderItem.php
│  │   ├─ Review.php
│  │   ├─ Cart.php   (pivotless service model)
│  │   └─ Address.php
│  ├─ Services/
│  │   ├─ CartService.php
│  │   ├─ PaymentService.php
│  │   ├─ NewsletterService.php
│  │   └─ TierService.php
│  ├─ Validation/
│  │   └─ CheckoutValidator.php
│  └─ Middlewares/
│      ├─ AuthMiddleware.php
│      ├─ AdminMiddleware.php
│      └─ CSRFMiddleware.php
├─ bootstrap/
│  └─ app.php
├─ config.php
├─ includes/
│  ├─ db.php
│  ├─ auth.php
│  └─ functions.php
├─ public/
│  ├─ index.php
│  ├─ .htaccess
│  └─ assets/
│      ├─ css/
│      │   └─ app.css
│      ├─ js/
│      │   └─ app.js
│      ├─ images/
│      └─ videos/
├─ routes/
│  ├─ web.php
│  └─ api.php
├─ resources/
│  └─ tailwind.config.js
├─ views/
│  ├─ layout/
│  │   ├─ master.php
│  │   ├─ header.php
│  │   └─ footer.php
│  ├─ home.php
│  ├─ product/
│  │   └─ show.php
│  ├─ cart/
│  │   └─ index.php
│  ├─ checkout/
│  │   └─ form.php
│  ├─ account/
│  │   ├─ login.php
│  │   └─ dashboard.php
│  └─ admin/
│      ├─ dashboard.php
│      └─ products.php
├─ storage/
│  ├─ logs/
│  └─ cache/
├─ vendor/ (Composer)
├─ .env.example
├─ composer.json
└─ package.json
```

*_Tip_: Place Apache vhost `DocumentRoot` at `public/` to avoid web access to `app/` & `includes/`.*

---

## 8. Configuration Management

### `/config.php`
```php
<?php
return [
  'app' => [
      'name'      => 'Luminaire Aromatherapy',
      'env'       => getenv('APP_ENV') ?: 'prod',
      'url'       => getenv('APP_URL') ?: 'https://lux-aroma.test',
      'debug'     => getenv('APP_DEBUG') === 'true',
  ],
  'db'  => [
      'driver'    => 'mysql',
      'host'      => getenv('DB_HOST') ?: '127.0.0.1',
      'port'      => getenv('DB_PORT') ?: 3306,
      'database'  => getenv('DB_DATABASE') ?: 'lux_aroma',
      'username'  => getenv('DB_USERNAME') ?: 'lux_user',
      'password'  => getenv('DB_PASSWORD') ?: 'secret',
      'charset'   => 'utf8mb4',
      'collation' => 'utf8mb4_unicode_ci',
  ],
  'mail' => [
      'from' => 'noreply@lux-aroma.com',
      'smtp_host' => getenv('SMTP_HOST'),
      'smtp_user' => getenv('SMTP_USER'),
      'smtp_pass' => getenv('SMTP_PASS'),
  ],
  'stripe' => [
      'pk' => getenv('STRIPE_PK'),
      'sk' => getenv('STRIPE_SK'),
  ],
];
```

### `/includes/db.php`
```php
<?php
$config = require __DIR__ . '/../config.php';
$db     = $config['db'];

$dsn = sprintf(
  '%s:host=%s;dbname=%s;port=%d;charset=%s',
  $db['driver'], $db['host'], $db['database'], $db['port'], $db['charset']
);

try {
    $pdo = new PDO($dsn, $db['username'], $db['password'], [
        PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    ]);
} catch (PDOException $e) {
    if ($config['app']['debug']) {
        throw $e;
    }
    error_log($e->getMessage());
    http_response_code(500);
    echo 'Database connection failed.';
    exit;
}
return $pdo;
```

All models access the DB through dependency-injecting this `$pdo` or via Eloquent (which uses the same DSN).

---

## 9. Database Schema & ER Diagram

### Core Tables

1. **users** (`id`, `name`, `email`, `password_hash`, `role`, `created_at`, `updated_at`, `deleted_at`)  
2. **products** (`id`, `category_id`, `slug`, `name`, `description`, `price_cents`, `inventory`, `thumbnail`, `is_active`)  
3. **categories** (`id`, `parent_id`, `slug`, `name`, `icon`)  
4. **orders** (`id`, `user_id`, `status`, `subtotal_cents`, `shipping_cents`, `tax_cents`, `total_cents`, `stripe_payment_intent`, `placed_at`)  
5. **order_items** (`id`, `order_id`, `product_id`, `qty`, `unit_price_cents`)  
6. **addresses** (`id`, `user_id`, `line1`, `line2`, `city`, `state`, `postal`, `country`, `type`)  
7. **reviews** (`id`, `product_id`, `user_id`, `rating`, `body`, `approved`)  
8. **cart_items** (`id`, `user_id`, `product_id`, `qty`)  
9. **newsletter_subscribers** (`id`, `email`, `subscribed_at`)  
10. **membership_tiers** (`id`, `user_id`, `tier`, `started_at`, `expires_at`)  

Use **InnoDB**, `utf8mb4`, foreign keys with `ON DELETE CASCADE`.

Index frequently queried columns: `products.slug`, `orders.user_id`, `reviews.product_id`.

### Migration Strategy
Write versioned migrations in `/database/migrations/*.php`:

```php
class CreateProductsTable
{
  public function up(PDO $db)
  {
    $sql = "CREATE TABLE products (
      id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
      category_id BIGINT UNSIGNED NOT NULL,
      slug VARCHAR(255) UNIQUE,
      name VARCHAR(255),
      description TEXT,
      price_cents INT UNSIGNED,
      inventory INT UNSIGNED,
      thumbnail VARCHAR(255),
      is_active TINYINT(1) DEFAULT 1,
      created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
      updated_at TIMESTAMP NULL,
      FOREIGN KEY (category_id) REFERENCES categories(id)
    ) ENGINE=InnoDB";
    $db->exec($sql);
  }
  public function down(PDO $db){ $db->exec("DROP TABLE products"); }
}
```

A CLI (`php artisan-lite migrate`) runs `up()` in order; `--rollback` executes `down()`.

---

## 10. Routing Map

| HTTP | URI | Controller@Method | Middleware |
|------|-----|-------------------|------------|
| GET  | `/` | HomeController@index | web |
| GET  | `/product/{slug}` | ProductController@show | web |
| GET  | `/cart` | CartController@index | web |
| POST | `/cart/add` | CartController@add | web |
| POST | `/cart/remove` | CartController@remove | web |
| GET  | `/checkout` | CheckoutController@form | auth,cartNotEmpty |
| POST | `/checkout` | CheckoutController@process | auth,csrf |
| GET  | `/account/login` | AccountController@loginForm | guest |
| POST | `/account/login` | AccountController@login | guest |
| POST | `/account/logout` | AccountController@logout | auth |
| GET  | `/admin/dashboard` | Admin\DashboardController@index | auth,admin |
| REST | `/api/v1/*` | JSON Controllers | sanctum |

2-factor tokens, rate limiting and CSRF are auto-attached by middlewares.

---

## 11. Controllers (Key Methods)

### 11.1 `ProductController`
```php
class ProductController
{
  public function __construct(private Product $productModel) {}

  public function show(string $slug)
  {
    $product = $this->productModel->where('slug', $slug)
                                   ->with(['category','reviews.user'])
                                   ->firstOrFail();
    return View::make('product/show', compact('product'));
  }
}
```

### 11.2 `CartController`
* `add(Request $req)`  
  * Validate `product_id` & `qty`  
  * `$this->cartService->add($productId, $qty, $userId)`  
  * Return JSON (`count`, `subtotal`) for AJAX update.

### 11.3 `CheckoutController`
* `form()` renders Tailwind checkout form with billing & shipping.  
* `process()`  
  1. Validate via `CheckoutValidator`.  
  2. Create **Stripe PaymentIntent** (server-side).  
  3. On success, persist `orders` & `order_items`.  
  4. Send confirmation email (queue).  
  5. Destroy cart.

---

## 12. Models (Important Snippets)

### 12.1 `Product.php`
```php
class Product extends Model
{
  use SoftDeletes;

  protected $fillable = [
    'category_id','slug','name','description',
    'price_cents','inventory','thumbnail','is_active'
  ];

  public function category(){ return $this->belongsTo(Category::class); }

  public function reviews(){ return $this->hasMany(Review::class); }

  public function getPriceAttribute(): string
  {
      return number_format($this->price_cents / 100, 2);
  }
}
```

### 12.2 `CartService.php`
```php
class CartService
{
  public function __construct(private PDO $db){}

  public function add(int $productId, int $qty, int $userId): void
  {
      $stmt = $this->db->prepare(
         'INSERT INTO cart_items (user_id,product_id,qty)
          VALUES (?,?,?)
          ON DUPLICATE KEY UPDATE qty = qty + VALUES(qty)'
      );
      $stmt->execute([$userId,$productId,$qty]);
  }

  public function items(int $userId): array
  {
      $sql = 'SELECT ci.*,p.name,p.thumbnail,p.price_cents
              FROM cart_items ci
              JOIN products p ON p.id = ci.product_id
              WHERE ci.user_id = ?';
      return $this->db->prepare($sql)->execute([$userId])->fetchAll();
  }
}
```

---

## 13. Views & Tailwind Workflow

1. **Tailwind** config in `/resources/tailwind.config.js`.  
   ```js
   export default {
     content: ["../views/**/*.php", "../public/assets/js/**/*.js"],
     theme: { extend: { colors: { accent: "#d4af37" } } },
     plugins: [require("@tailwindcss/typography")],
   }
   ```
2. Run `npx vite` to bundle `/public/assets/css/app.css` & `/public/assets/js/app.js`.  
3. Example partial: `/views/layout/header.php`
   ```php
   <header class="fixed w-full bg-white/30 backdrop-blur z-50">
     <div class="container mx-auto flex justify-between items-center py-3">
       <a href="/" class="font-cinzel tracking-wider">LUMINAIRE</a>
       <nav class="hidden md:flex gap-6">
         <a class="hover:text-accent" href="/#shop">Shop</a>
         <a class="hover:text-accent" href="/#about">About</a>
       </nav>
       <div class="flex items-center gap-4">
        ...
       </div>
     </div>
   </header>
   ```
4. Page template inherits master:

   `/views/product/show.php`
   ```php
   <?php $this->layout('layout/master', ['title' => $product->name]); ?>
   <section class="container mx-auto py-24">
      ...
   </section>
   ```

The rudimentary layout engine can be `league/plates` or a 20-line custom include wrapper.

---

## 14. Security Hardening

| Vector | Mitigation |
|--------|------------|
| SQL Injection | PDO prepared statements + Eloquent builder. |
| XSS | `esc()` helper on all outputs; CSP default-src 'self'. |
| CSRF | Double-submit cookies; token `<input name="_token">`. |
| Auth | BCrypt (`PASSWORD_BCRYPT`) 12 rounds, rate-limited login. |
| HTTPS | HSTS 1 year, secure cookies, `SameSite=Lax`. |
| RCE | `open_basedir`, `disable_functions=exec,system,passthru`. |
| File Uploads | Mime-type & extension whitelist, store outside webroot. |
| Admin Routes | `AdminMiddleware` restricts to role `admin`. |
| Payments | Stripe Elements keeps CC data off server. |
| Headers | `Referrer-Policy: strict-origin-when-cross-origin`, `X-Frame-Options: DENY`. |

---

## 15. Payment & Third-Party Integrations

1. **Stripe Checkout + Webhooks**  
   *GET `/checkout` → PaymentIntent client secret → Stripe.js*  
   *Webhook `/webhook/stripe` (order paid, refund created)* – verifies via signature.

2. **Mail** – SMTP (SendGrid) for transactional and marketing.

3. **Analytics** – Self-hosted Plausible, embedded via defer script.

4. **Search** – Optional MeiliSearch micro-service; Eloquent synonyms indexer.

---

## 16. Admin Back-Office

* `/admin/dashboard` – at-a-glance metrics (orders today, stock low, revenue).  
* `/admin/products` – CRUD with image upload (S3).  
* `/admin/orders` – Status workflow (`new → paid → fulfilled → closed`).  
* `/admin/cms` – WYSIWYG for blog posts & landing blocks (TipTap editor).  
* RBAC enforced through `role` column; gates via `AdminMiddleware`.

---

## 17. Testing Strategy

| Layer | Tool | Coverage |
|-------|------|----------|
| Unit PHP | PHPUnit 11 | Models, Services, Helpers. |
| Feature | PestPHP | End-to-end HTTP, middleware. |
| JS | Vitest | Alpine.js helpers. |
| Browser | Cypress | Cart → Checkout happy path, a11y audits. |
| CI | GitHub Actions | Lint (PHPStan max 7), Static Analysis, Tests. |

Aim for **80 %** line coverage, mutation tests on CartService.

---

## 18. CI/CD & Deployment

1. **Git** main → develop → feature/* branches.  
2. GitHub Action matrix (PHP 8.3, MariaDB) runs tests, builds CSS/JS artefacts.  
3. On `main` tag:  
   * build Docker image `lux-aroma:1.0.0`  
   * push to registry  
   * trigger Ansible playbook:  
     * `docker pull`  
     * `docker-compose up -d --scale app=3 --no-deps` (zero-downtime).  
4. **Env** variables (secret manager) injected at container start.

---

## 19. Performance & Observability

* **HTTP/2**, Brotli compression.  
* Session & config cache via **Redis** cluster.  
* **OpCache** 256 MB, JIT tracing.  
* Query monitor – Laravel Telescope lite sends to **Grafana Loki**.  
* Prometheus exporter for FPM: `php-fpm_exporter`.  
* Alert rules: p95 latency > 400 ms, error ratio > 1 %.

---

## 20. Future Extensibility & Road-Map

| Phase | Feature | Notes |
|-------|---------|-------|
| 1.1 | Multi-currency | IBAN fx rates once/day. |
| 1.2 | i18n (de, fr, jp) | JSON translation files, URL prefix. |
| 1.3 | Headless API (`/api/v2`) | GraphQL with Lighthouse PHP. |
| 2.0 | Subscription boxes | Recurring orders, Stripe Billing webhooks. |
| 2.1 | Loyalty program | Points ledger table, tier auto-upgrade. |
| 3.0 | Marketplace | Vendor onboarding, split payments (Stripe Connect). |

---

## 21. Appendix

### 21.1 Coding Conventions
* PSR-12, PHPDocs on all public methods.  
* Namespaces: `App\Models`, `App\Controllers`.  
* Avoid Helpers inside models – use Services.

### 21.2 `composer.json` (excerpt)
```json
{
  "require": {
    "php": "^8.3",
    "illuminate/database": "^12.0",
    "illuminate/events": "^12.0",
    "illuminate/http": "^12.0",
    "illuminate/routing": "^12.0",
    "illuminate/validation": "^12.0",
    "vlucas/phpdotenv": "^5.5",
    "symfony/var-dumper": "^7.0",
    "guzzlehttp/guzzle": "^7.8",
    "stripe/stripe-php": "^12.0"
  },
  "autoload": { "psr-4": { "App\\": "app/" } }
}
```

### 21.3 `.env.example`
```
APP_ENV=local
APP_URL=http://localhost
APP_DEBUG=true

DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lux_aroma
DB_USERNAME=root
DB_PASSWORD=

SMTP_HOST=smtp.mailtrap.io
SMTP_USER=...
SMTP_PASS=...

STRIPE_PK=pk_test_...
STRIPE_SK=sk_test_...
```

### 21.4 Helper Functions (`/includes/functions.php`)
```php
function esc(string $html): string
{
    return htmlspecialchars($html, ENT_QUOTES, 'UTF-8');
}
function price(int $cents): string
{
    return '$' . number_format($cents / 100, 2);
}
```

---

## Closing Notes
This document provides an end-to-end blueprint—architecture, folder structure, DB schema, build tooling, security, and more—so that even a junior developer can scaffold, iterate, and ship the premium Luminaire aromatherapy store with confidence. By leveraging lightweight Laravel components atop a transparent MVC micro-kernel, the team enjoys modern conveniences (Routing, Eloquent, Validation) while retaining full visibility and control over every line of code.
