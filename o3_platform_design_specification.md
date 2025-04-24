# Éclat Naturals – Premium Aromatherapy E-Commerce Platform  
### Comprehensive Technical Design Specification  

## 1. Executive Summary  

This document specifies, in painstaking detail, how to engineer a modular, secure, and extensible e-commerce platform dedicated to premium aromatherapy products.  The reference front-end (latest luxury landing page) informs the aesthetic, UX, and naming conventions, while this specification focuses on *back-end*, *infrastructure*, *data*, and *build* aspects.  The target reader is a junior–mid PHP developer who is familiar with Linux, Apache, and Composer but may not have led a green-field build before.  By following this guide the developer should be able to produce a fully functional MVP that can later scale into a flagship commerce solution.

---

## 2. Guiding Principles  

1. **Transparency over Black-Box** – avoid monolithic e-commerce engines in favour of an *MVC-inspired* skeleton that reveals every layer of the stack.  
2. **Security First** – 100 % HTTPS, robust input validation, prepared statements, CSRF tokens, Content Security Policy (CSP), automatic rate-limiting, and strict least-privilege MySQL user.  
3. **Clean Separation of Concerns** – controllers orchestrate, models persist, views present.  Business logic stays in service classes; SQL stays in model classes; HTML stays in Blade-style views.  
4. **Performance by Default** – server-side caching, HTTP/2, optimized asset pipeline (Tailwind JIT + PurgeCSS), lazy-loading media, and eager database indices.  
5. **Extensibility** – every module (e.g., “CartService”, “PaymentService”) is a discreet PHP class exposed by an interface and registered in a lightweight service container.  
6. **Accessibility & UX Parity** – all dynamic UI additions preserve keyboard navigation, ARIA roles, colour-contrast ratios, and respectful reduced-motion fall-backs.  
7. **12-Factor Compliance** – configuration in `config.php` & `.env`, stateless application servers, logs printed to STDOUT, etc.  

---

## 3. Technology Stack  

| Layer | Selected Tech | Rationale |
|-------|---------------|-----------|
| HTTP Server | Apache 2.4 (prefork or event MPM) | Widely supported, robust mod_rewrite, easy virtual hosts. |
| Application | PHP 8.3 (FPM) | Latest stable features (readonly classes, `json_validate`, better enums, Fibers). |
| Micro-Framework | Custom MVC “kernel” + **Laravel 12 components only** (Routing, Blade, Validation) | We *extract* just the composer packages we need; no full Laravel bootstrap. |
| DB | MariaDB 11.7 | Drop-in MySQL replacement, better licensing, faster JSON, compatible with Laravel’s Eloquent. |
| Front-End | Tailwind CSS 3 (JIT), Alpine.js 3 for micro-interactions | Utility first, low payload; Alpine gives progressive-enhancement JS. |
| Build | Composer, npm (vite), PurgeCSS, postcss-cli | Tree-shake CSS, bundle + minify JS. |
| Hosting | Ubuntu 22.04 LTS on AWS Lightsail or similar | Modern TLS, systemd, automatic security updates. |
| Version Ctrl | git + GitHub | PR workflow, Actions CI/CD. |
| Container | optional: Docker Compose for parity between dev / CI / prod |   |

---

## 4. Environment & Local Setup  

1. **Clone the repository**  
   ```bash
   git clone git@github.com:eclat-naturals/ecommerce-platform.git
   cd ecommerce-platform
   ```

2. **System packages (Ubuntu example)**  
   ```bash
   sudo apt update && sudo apt install -y \
     apache2 libapache2-mod-fcgid \
     mariadb-server \
     php8.3 php8.3-fpm php8.3-mysql php8.3-xml php8.3-mbstring php8.3-curl \
     composer nodejs npm
   ```

3. **Composer install**  
   ```bash
   composer install --no-dev --optimize-autoloader
   ```

4. **Node / Tailwind build**  
   ```bash
   npm ci
   npm run dev   # or npm run build for production
   ```

5. **MariaDB user**  
   ```sql
   CREATE DATABASE eclat_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   CREATE USER 'eclat_app'@'localhost' IDENTIFIED BY 'ChangeMe!';
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER ON eclat_db.* TO 'eclat_app'@'localhost';
   FLUSH PRIVILEGES;
   ```

6. **Apache virtual host** (`/etc/apache2/sites-available/eclat.conf`)  
   ```apache
   <VirtualHost *:80>
       ServerName eclat.local
       DocumentRoot /var/www/eclat/public
       <Directory /var/www/eclat/public>
            AllowOverride All
            Require all granted
       </Directory>
       ErrorLog ${APACHE_LOG_DIR}/eclat_error.log
       CustomLog ${APACHE_LOG_DIR}/eclat_access.log combined
   </VirtualHost>
   ```
   Then `sudo a2ensite eclat && sudo a2enmod rewrite headers proxy_fcgi && sudo systemctl reload apache2`.

---

## 5. Directory & File Structure  

```
/ecommerce-platform
│
├── .env.example
├── composer.json
├── package.json
├── vite.config.js
├── public/
│   ├── index.php
│   ├── .htaccess
│   ├── css/
│   │   └── app.css          (compiled Tailwind)
│   ├── js/
│   │   └── app.js           (Alpine bootstrap)
│   ├── images/
│   │   ├── logo.svg
│   │   ├── hero-day.webp
│   │   └── hero-night.webp
│   └── videos/
│       └── ambience.mp4
│
├── includes/
│   ├── db.php
│   ├── autoload.php
│   ├── helpers.php
│   └── csrf.php
│
├── config.php
│
├── routes/
│   ├── web.php
│   └── api.php
│
├── app/
│   ├── Core/
│   │   ├── Controller.php
│   │   ├── Model.php
│   │   ├── View.php
│   │   ├── Router.php
│   │   ├── ServiceContainer.php
│   │   └── Middleware/
│   │       ├── AuthMiddleware.php
│   │       └── VerifyCsrfToken.php
│   │
│   ├── Controllers/
│   │   ├── HomeController.php
│   │   ├── ProductController.php
│   │   ├── CartController.php
│   │   ├── CheckoutController.php
│   │   ├── AuthController.php
│   │   └── Admin/
│   │       ├── DashboardController.php
│   │       └── ProductController.php
│   │
│   ├── Models/
│   │   ├── User.php
│   │   ├── Product.php
│   │   ├── Category.php
│   │   ├── Order.php
│   │   ├── OrderItem.php
│   │   └── Review.php
│   │
│   ├── Services/
│   │   ├── CartService.php
│   │   ├── PaymentService.php
│   │   ├── MailerService.php
│   │   ├── ThemeService.php
│   │   └── DiscountService.php
│   │
│   └── Policies/
│       └── ProductPolicy.php
│
├── views/
│   ├── layout/
│   │   ├── master.blade.php
│   │   ├── header.blade.php
│   │   └── footer.blade.php
│   ├── home.blade.php
│   ├── product/
│   │   ├── index.blade.php
│   │   └── show.blade.php
│   ├── cart.blade.php
│   ├── checkout.blade.php
│   ├── auth/
│   │   ├── login.blade.php
│   │   └── register.blade.php
│   └── admin/
│       ├── dashboard.blade.php
│       └── products.blade.php
│
└── tests/
    ├── Unit/
    └── Feature/
```

### Explanation of Key Paths  

| Directory | Purpose |
|-----------|---------|
| `/public` | Web-root only.  All requests funneled through `public/index.php` (front controller). |
| `/includes` | Low-level utilities shared by both CLI and web.  Eg. `db.php` returns a PDO instance. |
| `/app/Core` | Mini-framework kernel: routing, base controller/model, DI container, middlewares. |
| `/app/Controllers` | HTTP request handlers.  1 file per resource or grouping. |
| `/app/Models` | Eloquent-style ActiveRecord classes. |
| `/app/Services` | Reusable business logic; thin controllers call services. |
| `/views` | Blade templates.  All UI lives here; never mix PHP control flow in HTML beyond `@directives`. |
| `/tests` | PHPUnit tests wired through GitHub Actions. |

---

## 6. Configuration & Environment Management  

1. **`config.php`** – PHP array returning non-secret constants.  
   ```php
   <?php
   return [
     'app_name'      => 'Éclat Naturals',
     'base_url'      => getenv('APP_URL') ?: 'http://localhost',
     'assets_url'    => getenv('APP_URL') . '/public',
     'timezone'      => 'UTC',
     'session_life'  => 7200,
     'theme_default' => 'day',
   ];
   ```

2. **`.env`** (never committed)  
   ```
   APP_ENV=development
   APP_URL=http://eclat.local
   DB_HOST=localhost
   DB_DATABASE=eclat_db
   DB_USERNAME=eclat_app
   DB_PASSWORD=ChangeMe!
   MAIL_HOST=smtp.mailtrap.io
   MAIL_PORT=2525
   MAIL_USERNAME=mailuser
   MAIL_PASSWORD=Secret
   STRIPE_SK=test_...
   ```
3. **`includes/db.php`**  
   ```php
   <?php
   $dotenv = Dotenv\Dotenv::createImmutable(dirname(__DIR__));
   $dotenv->load();

   try {
       $pdo = new PDO(
           'mysql:host='. $_ENV['DB_HOST'] .';dbname='. $_ENV['DB_DATABASE'] .';charset=utf8mb4',
           $_ENV['DB_USERNAME'],
           $_ENV['DB_PASSWORD'],
           [
             PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
             PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
             PDO::ATTR_EMULATE_PREPARES   => false,
           ]
       );
   } catch (PDOException $e) {
       http_response_code(500);
       echo 'DB Connection failed.';
       error_log($e->getMessage());
       exit;
   }

   return $pdo;
   ```

All subsequent model constructors require `global $pdo;` or resolve via the ServiceContainer.

---

## 7. Core Framework Components  

### 7.1 Router  

* Simple route table defined in `/routes/web.php`.  
* RegEx-based matching; supports GET, POST, PUT, DELETE.  
* Auto CSRF middleware on unsafe verbs.  

```php
Router::get('/', [HomeController::class, 'index']);
Router::get('/product/{slug}', [ProductController::class, 'show']);
Router::post('/cart/add', [CartController::class, 'store'])->middleware('auth');
```

### 7.2 Controller Base  

```php
abstract class Controller {
    protected function view($path, $data = []) {
        return View::render($path, $data);
    }
    protected function redirect($url) {
        header("Location: {$url}", true, 302); exit;
    }
}
```

### 7.3 View Engine  

We leverage Laravel’s `blade` standalone composer package:

```php
$views = __DIR__.'/../../views';
$cache = __DIR__.'/../../storage/cache';

$blade = new Jenssegers\Blade\Blade($views, $cache);
```

### 7.4 Service Container  

A 100-line PSR-11 implementation allows easy dependency injection:

```php
$container->bind(CartService::class, fn() => new CartService());
$container->singleton(PDO::class, fn() => require __DIR__.'/../../includes/db.php');
```

Controllers request dependencies via constructor type-hints; the container resolves them.

---

## 8. Database Design & Schema  

### 8.1 ER Diagram (text description)

* `users` (id PK, name, email, password_hash, role, created_at, updated_at)  
* `categories` (id PK, name, slug, parent_id FK)  
* `products` (id PK, category_id FK, name, slug, description_md, price_cents, stock, featured, hero_img, thumb_img, created_at, updated_at)  
* `product_images` (id PK, product_id FK, path, alt_text, sort_order)  
* `reviews` (id PK, product_id FK, user_id FK, rating_int, title, body, created_at)  
* `carts` (id PK, user_id FK nullable, session_key, created_at, updated_at)  
* `cart_items` (cart_id FK, product_id FK, qty) (composite PK)  
* `orders` (id PK, user_id FK, subtotal_cents, tax_cents, total_cents, currency, stripe_payment_id, status_enum, created_at)  
* `order_items` (order_id FK, product_id FK, price_cents, qty)  
* `password_resets` (user_id FK, token_hash, created_at)  

### 8.2 SQL Snippet (products)

```sql
CREATE TABLE `products` (
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `category_id` INT UNSIGNED,
  `name` VARCHAR(120) NOT NULL,
  `slug` VARCHAR(140) UNIQUE,
  `description_md` MEDIUMTEXT,
  `price_cents` INT UNSIGNED NOT NULL,
  `stock` SMALLINT UNSIGNED DEFAULT 0,
  `featured` TINYINT(1) DEFAULT 0,
  `hero_img` VARCHAR(255),
  `thumb_img` VARCHAR(255),
  `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  `updated_at` TIMESTAMP NULL ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB;
```

### 8.3 PDO Model Example

```php
class Product extends Model {
   protected $table = 'products';
   public function findBySlug(string $slug): ?array {
       $sql = "SELECT * FROM {$this->table} WHERE slug = :slug LIMIT 1";
       $stmt = $this->pdo->prepare($sql);
       $stmt->execute(['slug'=>$slug]);
       return $stmt->fetch() ?: null;
   }
}
```

---

## 9. Module-By-Module Breakdown  

### 9.1 Product Catalogue  

* **URL**: `/products` (index), `/product/{slug}` (detail).  
* Search by keywords & category; optional price slider (client-side).  
* Detail page loads hero image, gallery, fragrance notes, ingredient spotlight, reviews.  
* SEO: `schema.org/Product` microdata, OpenGraph tags.  

### 9.2 Cart  

* Sessions for guests (PHP `session_id()`); persisted row for authenticated users.  
* Store only `product_id + quantity`; price revalidated at checkout time.  
* `CartService` public API: `add`, `remove`, `updateQty`, `total()`, `items()`.  
* Alpine.js calls `/cart/add` via fetch; server returns JSON (#items, mini-cart HTML fragment).  

### 9.3 Checkout  

| Step | View | Controller Action | Notes |
|------|------|-------------------|-------|
| 1 | `/checkout` | `CheckoutController@create` | Address & shipping method forms; CSRF token. |
| 2 | `/checkout/payment` | `CheckoutController@process` | Stripe Payment-Intent; JS `stripe.js` load. |
| 3 | `/checkout/complete/{order}` | `CheckoutController@success` | Thank-you page; order summary; GA4 event. |

* CSRF + Recaptcha on address forms.  
* Realtime shipping rates stubbed for MVP (flat $9).  

### 9.4 Authentication  

* Passwords hashed with Argon2id (PHP 8.3 default).  
* Email verification tokens stored in `password_resets`.  
* Rate-limited login (5 attempts / 15 min per IP).  

### 9.5 Admin Panel  

* `/admin` area, protected by role middleware (`role == 'admin'`).  
* CRUD for Products, Categories, Orders, Users.  
* Uses reusable Vue-powered tables inside Blade.  

### 9.6 ThemeService (Day/Night)  

* Writes cookie `theme=night|day; Max-Age=31536000; SameSite=Lax`.  
* Middleware sets `$_SESSION['theme']`.  
* `ThemeService->current()` returns `'day'` fallback.  
* CSS variables toggled in `<html class="theme-night">`.  

---

## 10. Security Hardening Checklist  

1. **Input Validation**  
   * Laravel Validation facade for complex rules.  
   * `filter_var()` for emails, ints, URLs.  

2. **Prepared Statements**  
   * Absolutely no string-concatenated SQL.  

3. **CSRF**  
   * Hidden `_token` in every POST form.  
   * Verify token in `VerifyCsrfToken` middleware.  

4. **XSS**  
   * Blade templates escape by default `{{ $var }}`.  
   * Trusted HTML (e.g., product description) passes through HTMLPurifier.  

5. **Session Security**  
   * `session.cookie_secure = 1`, `session.cookie_httponly = 1`, `SameSite=Lax`.  
   * Regenerate ID on login.  

6. **Content Security Policy**  

```
default-src 'self';
img-src 'self' data: https: blob:;
script-src 'self' 'nonce-{nonce}';
style-src 'self' 'unsafe-inline';
```

7. **Permissions & Ownership**  
   * All PHP files 644, dirs 755.  
   * `storage/` + `cache/` writable by www-data, not others.  

8. **Password Policy**  
   * Min 10 chars, 1 uppercase, 1 number, 1 symbol.  
   * Forgot-password link expires after 30 min.  

9. **Audit & Logging**  
   * Log all admin logins, product edits, payment failures.  
   * Use `Monolog` to ship to CloudWatch or ELK.  

10. **Automated Scans**  
    * GitHub Dependabot for Composer + npm packages.  
    * OWASP ZAP monthly pipeline.  

---

## 11. Build & Asset Pipeline  

1. **Tailwind CSS**  
   * `tailwind.config.js` extends brand palette:  
     ```js
     module.exports = {
       content: ["./views/**/*.blade.php", "./public/js/**/*.js"],
       theme: {
         extend: {
           colors: {
             accent: '#d4af37',
             nightaccent: '#50d890',
           }
         }
       }
     }
     ```  
   * JIT builds after each save (`npm run dev`).  
   * Production: `npm run build` (vite + PostCSS) → hashes `app.[hash].css`.  

2. **JavaScript**  
   * *No heavy front-end framework.*  
   * `Alpine.js` loads by CDN with `defer`.  
   * Only ~10 kB after gzip.  

3. **Images & Video**  
   * `.webp` for hero and catalog thumbnails.  
   * `srcset` responsive images.  
   * Lazy loaded `<img loading="lazy">`.  

---

## 12. Routing & URL Patterns  

| Resource | Pattern | Named Route | Controller |
|----------|---------|-------------|------------|
| Home | `/` | `home` | HomeController@index |
| Category | `/category/{slug}` | `category.show` | ProductController@category |
| Product | `/product/{slug}` | `product.show` | ProductController@show |
| Cart Add | `/cart/add` (POST) | `cart.add` | CartController@store |
| Checkout | `/checkout` | `checkout` | CheckoutController@create |
| API Products | `/api/products` | `api.products` | Api\ProductController@index |

`routes/api.php` returns JSON only; uses stateless token (Bearer) authentication.

---

## 13. Testing Strategy  

1. **Unit Tests** (`tests/Unit`)  
   * Model methods: price helpers, slug generation, `Product::findBySlug`.  
   * Services: `CartService::total()`, `DiscountService::apply()`.  

2. **Feature Tests** (`tests/Feature`)  
   * HTTP endpoints: GET `/product/lavender-dream-balm` returns 200.  
   * CSRF token required on POST `/cart/add`.  

3. **Browser Tests** (optional)  
   * Laravel Dusk or Cypress for end-to-end checkout flow.  

4. **CI/CD** – GitHub Actions  
   ```yaml
   name: CI
   on: [push]
   jobs:
     test:
       runs-on: ubuntu-latest
       services:
         mysql:
           image: mariadb:11.7
           env:
             MYSQL_ROOT_PASSWORD: root
             MYSQL_DATABASE: eclat_test
           ports: ['3306:3306']
           options: >-
             --health-cmd="mysqladmin ping --silent"
             --health-interval=10s
             --health-timeout=5s
             --health-retries=3
       steps:
         - uses: actions/checkout@v4
         - uses: php-actions/composer@v6
         - run: composer install --no-interaction
         - run: vendor/bin/phpunit
   ```

---

## 14. Logging & Monitoring  

* **Monolog** channels: `daily` (rotating 14 days), `stderr`, `slack`.  
* Apache access logs parsed by GoAccess for real-time dashboard.  
* `HealthController@status` returns 200 JSON heartbeat; used by AWS ALB.  
* `prometheus.php` metrics endpoint if scaling horizontally.

---

## 15. Deployment Playbook  

1. **Build artefacts** – CI pipeline generates `/dist` assets.  
2. **SSH** into server (key only).  
3. **Pull** from Git, run `composer install --no-dev --optimize-autoloader`.  
4. **Run DB migrations** (artisan-like CLI script).  
5. **Cache Busting** – `php artisan config:cache` equivalent; copy `.env`.  
6. **Restart** PHP-FPM (`systemctl restart php8.3-fpm`).  
7. **Clear opcache** via `opcache_reset()` call in admin.  

---

## 16. Future Extensions  

| Feature | Rationale | Implementation Sketch |
|---------|-----------|------------------------|
| Multi-Currency | Expand EU & APAC reach | `prices` table with currency code, exchange update cron job. |
| Gift Messages | Aromatherapy is a gifting product category | `order_gifts` table; optional message at checkout. |
| Headless API | Mobile app, JAMstack front-end | Build GraphQL endpoint with `webonyx/graphql-php`. |
| Subscriptions | Essential oil refills | Integrate Stripe Billing; `subscriptions` table. |
| ElasticSearch | Faceted search, synonyms for botanical names | Use `elastic/elasticsearch-php` 8.x client. |

---

## 17. Glossary  

* **MVC** – Model View Controller.  
* **Blade** – Laravel’s PHP templating engine.  
* **JIT** – Just-In-Time compilation (Tailwind).  
* **CSRF** – Cross-Site Request Forgery.  
* **CSP** – Content Security Policy.  
* **MVP** – Minimum Viable Product.  

---

## 18. Appendix A – Example `.htaccess`  

```apache
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)$ index.php [QSA,L]
</IfModule>

Header always set Referrer-Policy "strict-origin-when-cross-origin"
Header always set X-Content-Type-Options "nosniff"
Header always set X-Frame-Options "SAMEORIGIN"
```

---

## 19. Appendix B – Minimal `index.php` Front Controller  

```php
require dirname(__DIR__).'/vendor/autoload.php';
$config   = require dirname(__DIR__).'/config.php';
$router   = require dirname(__DIR__).'/app/Core/bootstrap.php';

$router->dispatch(
    $_SERVER['REQUEST_METHOD'],
    parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH)
);
```

---

## 20. Concluding Notes  

By adhering to this blueprint you will craft a robust e-commerce architecture that balances **luxury brand storytelling** with hardened engineering practices.  The file structure is intentionally verbose so onboarding juniors can “open any door and see exactly what lives inside.”  Meanwhile, senior developers retain the freedom to swap in heavier frameworks, container-orchestration, or microservices down the road.

Happy coding—and may your users inhale the finest digital aromas of Éclat Naturals.
