# E-Commerce Platform Technical Design Specification Document
## LUMIÈRE Premium Aromatherapy E-Commerce Platform

**Version 2.0**
**Date: April 24, 2025**

---

## 1. Executive Summary

This technical design specification document outlines the architecture, design patterns, database schema, and implementation details for building a premium, modular e-commerce platform specifically designed for the LUMIÈRE aromatherapy brand. The platform will deliver a luxury shopping experience for customers seeking high-quality essential oils, bath products, and aromatherapy accessories while providing a robust, secure, and scalable backend for administrators.

The platform leverages a custom PHP MVC-inspired architecture based on Laravel 12 principles but with a lightweight implementation that maximizes transparency and developer control. This document provides comprehensive guidelines to ensure consistent development practices, security measures, and future extensibility.

---

## 2. Project Overview

### 2.1 Project Objectives

- Create a visually stunning e-commerce platform that embodies the luxury essence of the LUMIÈRE brand
- Implement a secure, scalable, and maintainable codebase using modern PHP 8.3 features
- Design a modular architecture that allows for easy feature additions and modifications
- Optimize for performance with appropriate caching strategies and lazy loading
- Ensure comprehensive security measures against common web vulnerabilities
- Build responsive interfaces that work seamlessly across all devices

### 2.2 Key Features

- Sophisticated product catalog with advanced filtering and search capabilities
- Secure user authentication and profile management
- Streamlined checkout process with multiple payment options
- Order management and tracking
- Customer reviews and ratings system
- Wish list functionality
- Customizable product recommendations
- Admin dashboard for inventory, order, and customer management
- Multi-language and multi-currency support
- Analytics integration
- Email notification system

---

## 3. Technology Stack

### 3.1 Server Environment

- Web Server: Apache 2.4+
- PHP Version: 8.3+
- Database: MariaDB 11.7+
- Caching: Redis 7.0+ (optional for performance enhancement)

### 3.2 Frontend Technologies

- HTML5
- CSS3 with Tailwind CSS 3.3+
- JavaScript (ES6+)
- Alpine.js 3.12+ for interactive components
- GSAP for advanced animations
- Vanilla Lazyload for image optimization

### 3.3 Backend Framework

- Custom MVC architecture inspired by Laravel 12
- Composer for dependency management
- PHPMailer for email functionality
- Monolog for logging
- Intervention Image for image processing

### 3.4 Development Tools

- Git for version control
- PHPUnit for unit testing
- PHPCS for code style enforcement
- NPM for frontend asset management
- Laravel Mix for asset compilation

---

## 4. System Architecture

### 4.1 MVC Architecture Overview

The platform follows a custom implementation of the Model-View-Controller (MVC) pattern, ensuring separation of concerns:

- **Models**: Represent data structures and business logic, handling database interactions
- **Views**: Handle the presentation layer, rendering HTML templates
- **Controllers**: Process user input, interact with models, and determine which view to render

Additionally, we'll implement:

- **Services**: Handle complex business logic that spans multiple models
- **Repositories**: Abstract database operations from the models
- **Helpers**: Provide utility functions used throughout the application
- **Middleware**: Handle request/response filtering, such as authentication checks

### 4.2 Directory Structure

```
/public_html                      # Web root directory
|-- /css                          # Compiled CSS files
|   |-- main.css                  # Main stylesheet
|   |-- admin.css                 # Admin-specific styles
|   |-- checkout.css              # Checkout-specific styles
|-- /js                           # JavaScript files
|   |-- main.js                   # Main JavaScript bundle
|   |-- admin.js                  # Admin functionality
|   |-- product.js                # Product page functionality
|-- /images                       # Static images
|   |-- /products                 # Product images
|   |-- /banners                  # Banner images
|   |-- /icons                    # Icon assets
|-- /videos                       # Video assets
|   |-- hero-background.mp4       # Hero section background video
|   |-- product-tutorials/        # Product usage tutorials
|-- /uploads                      # User-uploaded content (protected)
|-- /fonts                        # Web fonts
|-- index.php                     # Application entry point
|-- .htaccess                     # URL rewriting and security rules

/app                              # Application code (outside web root)
|-- /config                       # Configuration files
|   |-- config.php                # Main configuration file
|   |-- database.php              # Database configuration
|   |-- mail.php                  # Email configuration
|   |-- payment.php               # Payment gateway configuration
|-- /includes                     # Core files and utilities
|   |-- db.php                    # Database connection handler
|   |-- functions.php             # Global helper functions
|   |-- constants.php             # Application constants
|   |-- autoloader.php            # Class autoloader
|   |-- session.php               # Session management
|-- /controllers                  # Controller classes
|   |-- BaseController.php        # Base controller class
|   |-- HomeController.php        # Homepage controller
|   |-- ProductController.php     # Product management
|   |-- CartController.php        # Shopping cart operations
|   |-- CheckoutController.php    # Checkout process
|   |-- UserController.php        # User account management
|   |-- OrderController.php       # Order management
|   |-- AdminController.php       # Admin area controllers
|   |-- Api/                      # API controllers
|-- /models                       # Model classes
|   |-- BaseModel.php             # Base model class
|   |-- User.php                  # User model
|   |-- Product.php               # Product model
|   |-- Category.php              # Category model
|   |-- Order.php                 # Order model
|   |-- OrderItem.php             # Order item model
|   |-- Review.php                # Product review model
|   |-- Payment.php               # Payment model
|-- /services                     # Service classes
|   |-- AuthService.php           # Authentication service
|   |-- CartService.php           # Shopping cart service
|   |-- PaymentService.php        # Payment processing service
|   |-- EmailService.php          # Email service
|   |-- ImageService.php          # Image processing service
|-- /repositories                 # Repository classes
|   |-- BaseRepository.php        # Base repository class
|   |-- UserRepository.php        # User data operations
|   |-- ProductRepository.php     # Product data operations
|-- /middleware                   # Middleware classes
|   |-- Authenticate.php          # Authentication middleware
|   |-- AdminAuth.php             # Admin authentication
|   |-- CSRF.php                  # CSRF protection
|-- /views                        # View templates
|   |-- /layout                   # Layout templates
|   |   |-- master.php            # Main layout template
|   |   |-- admin.php             # Admin layout template
|   |-- /home                     # Homepage views
|   |   |-- index.php             # Homepage template
|   |   |-- featured.php          # Featured products section
|   |-- /products                 # Product views
|   |   |-- listing.php           # Product listing template
|   |   |-- detail.php            # Product detail template
|   |   |-- reviews.php           # Product reviews section
|   |-- /cart                     # Cart views
|   |   |-- index.php             # Cart page template
|   |   |-- mini.php              # Mini cart template
|   |-- /checkout                 # Checkout views
|   |   |-- index.php             # Checkout page
|   |   |-- payment.php           # Payment step
|   |   |-- confirmation.php      # Order confirmation
|   |-- /user                     # User account views
|   |   |-- login.php             # Login form
|   |   |-- register.php          # Registration form
|   |   |-- profile.php           # User profile page
|   |   |-- orders.php            # Order history
|   |-- /admin                    # Admin dashboard views
|   |   |-- dashboard.php         # Admin dashboard
|   |   |-- products/             # Product management views
|   |   |-- orders/               # Order management views
|   |   |-- users/                # User management views
|-- /tests                        # Test files
|-- /migrations                   # Database migrations
|-- /seed                         # Database seeders
|-- bootstrap.php                 # Application bootstrap file
|-- routes.php                    # Route definitions

/vendor                           # Composer dependencies
/node_modules                     # NPM dependencies
/logs                             # Application logs
/storage                          # File storage
```

### 4.3 Entry Point & Bootstrapping

The `index.php` file serves as the application entry point and will handle:

1. Loading the autoloader
2. Loading environment configuration
3. Bootstrapping the application
4. Routing the request to the appropriate controller
5. Sending the response

**File: public_html/index.php**
```php
<?php
// Define application path constants
define('APP_ROOT', dirname(__DIR__) . '/app');
define('PUBLIC_ROOT', __DIR__);

// Load bootstrap file
require_once APP_ROOT . '/bootstrap.php';

// Initialize the application
$app = new App\Core\Application();

// Process the request
$app->processRequest();
```

**File: app/bootstrap.php**
```php
<?php
// Load autoloader
require_once __DIR__ . '/../vendor/autoload.php';

// Load configuration
$config = require_once __DIR__ . '/config/config.php';

// Set error reporting based on environment
if ($config['environment'] === 'development') {
    error_reporting(E_ALL);
    ini_set('display_errors', 1);
} else {
    error_reporting(0);
    ini_set('display_errors', 0);
}

// Initialize session
require_once __DIR__ . '/includes/session.php';

// Load route definitions
require_once __DIR__ . '/routes.php';
```

---

## 5. Database Design

### 5.1 Database Schema

The database schema will consist of the following core tables:

1. **users** - User account information
2. **user_addresses** - User shipping/billing addresses
3. **products** - Product details
4. **product_categories** - Product categories
5. **product_images** - Product images
6. **product_attributes** - Product attributes (size, color, etc.)
7. **product_inventory** - Product inventory tracking
8. **categories** - Product categories hierarchy
9. **orders** - Order information
10. **order_items** - Items in each order
11. **order_statuses** - Order status definitions
12. **payments** - Payment transaction details
13. **reviews** - Product reviews
14. **carts** - Shopping cart data
15. **cart_items** - Items in shopping carts
16. **wishlist** - User wishlist items
17. **coupons** - Discount coupon codes
18. **tax_rates** - Tax rates by region
19. **shipping_methods** - Available shipping methods
20. **page_content** - CMS content for static pages

### 5.2 Entity Relationship Diagram

![Database ERD would be displayed here]

### 5.3 Table Definitions

**users**
```sql
CREATE TABLE users (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    role ENUM('customer', 'admin', 'editor') DEFAULT 'customer',
    is_active BOOLEAN DEFAULT TRUE,
    email_verified_at TIMESTAMP NULL,
    remember_token VARCHAR(100),
    reset_token VARCHAR(100),
    reset_token_expiry TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    last_login TIMESTAMP NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

**products**
```sql
CREATE TABLE products (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    sku VARCHAR(50) NOT NULL UNIQUE,
    name VARCHAR(255) NOT NULL,
    slug VARCHAR(255) NOT NULL UNIQUE,
    description TEXT,
    short_description VARCHAR(500),
    price DECIMAL(10, 2) NOT NULL,
    sale_price DECIMAL(10, 2),
    cost DECIMAL(10, 2),
    quantity INT UNSIGNED DEFAULT 0,
    is_featured BOOLEAN DEFAULT FALSE,
    is_active BOOLEAN DEFAULT TRUE,
    meta_title VARCHAR(255),
    meta_description VARCHAR(500),
    category_id INT UNSIGNED,
    brand_id INT UNSIGNED,
    tax_class_id INT UNSIGNED,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE SET NULL,
    FOREIGN KEY (brand_id) REFERENCES brands(id) ON DELETE SET NULL,
    FOREIGN KEY (tax_class_id) REFERENCES tax_classes(id) ON DELETE SET NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

**orders**
```sql
CREATE TABLE orders (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    order_number VARCHAR(50) NOT NULL UNIQUE,
    user_id INT UNSIGNED,
    status_id INT UNSIGNED NOT NULL,
    shipping_address_id INT UNSIGNED,
    billing_address_id INT UNSIGNED,
    shipping_method_id INT UNSIGNED,
    payment_method_id INT UNSIGNED,
    subtotal DECIMAL(10, 2) NOT NULL,
    shipping_cost DECIMAL(10, 2) NOT NULL DEFAULT 0,
    tax_amount DECIMAL(10, 2) NOT NULL DEFAULT 0,
    discount_amount DECIMAL(10, 2) NOT NULL DEFAULT 0,
    total DECIMAL(10, 2) NOT NULL,
    notes TEXT,
    tracking_number VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL,
    FOREIGN KEY (status_id) REFERENCES order_statuses(id),
    FOREIGN KEY (shipping_address_id) REFERENCES addresses(id) ON DELETE SET NULL,
    FOREIGN KEY (billing_address_id) REFERENCES addresses(id) ON DELETE SET NULL,
    FOREIGN KEY (shipping_method_id) REFERENCES shipping_methods(id) ON DELETE SET NULL,
    FOREIGN KEY (payment_method_id) REFERENCES payment_methods(id) ON DELETE SET NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### 5.4 Database Connection

**File: app/config/database.php**
```php
<?php
return [
    'driver' => 'mysql',
    'host' => 'localhost',
    'port' => 3306,
    'database' => 'lumiere_db',
    'username' => 'lumiere_user',
    'password' => 'secure_password_here',
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix' => '',
    'options' => [
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
        PDO::ATTR_EMULATE_PREPARES => false,
    ],
];
```

**File: app/includes/db.php**
```php
<?php
namespace App\Includes;

use PDO;
use PDOException;

class Database {
    private static $instance = null;
    private $connection;
    private $statement;

    private function __construct() {
        $config = require APP_ROOT . '/config/database.php';
        
        $dsn = "{$config['driver']}:host={$config['host']};port={$config['port']};dbname={$config['database']};charset={$config['charset']}";
        
        try {
            $this->connection = new PDO($dsn, $config['username'], $config['password'], $config['options']);
        } catch (PDOException $e) {
            // Log error and display user-friendly message
            error_log("Database connection failed: " . $e->getMessage());
            die("We're experiencing technical difficulties. Please try again later.");
        }
    }

    // Singleton pattern to ensure only one database connection
    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }

    // Prepare statement with query
    public function query($sql) {
        $this->statement = $this->connection->prepare($sql);
        return $this;
    }

    // Bind values to prepared statement
    public function bind($param, $value, $type = null) {
        if (is_null($type)) {
            switch (true) {
                case is_int($value):
                    $type = PDO::PARAM_INT;
                    break;
                case is_bool($value):
                    $type = PDO::PARAM_BOOL;
                    break;
                case is_null($value):
                    $type = PDO::PARAM_NULL;
                    break;
                default:
                    $type = PDO::PARAM_STR;
            }
        }
        
        $this->statement->bindValue($param, $value, $type);
        return $this;
    }

    // Execute the prepared statement
    public function execute() {
        try {
            return $this->statement->execute();
        } catch (PDOException $e) {
            error_log("Query execution failed: " . $e->getMessage());
            return false;
        }
    }

    // Get result set as array of objects
    public function resultSet() {
        $this->execute();
        return $this->statement->fetchAll();
    }

    // Get single record as object
    public function single() {
        $this->execute();
        return $this->statement->fetch();
    }

    // Get row count
    public function rowCount() {
        return $this->statement->rowCount();
    }

    // Get last inserted ID
    public function lastInsertId() {
        return $this->connection->lastInsertId();
    }

    // Start transaction
    public function beginTransaction() {
        return $this->connection->beginTransaction();
    }

    // Commit transaction
    public function commit() {
        return $this->connection->commit();
    }

    // Rollback transaction
    public function rollBack() {
        return $this->connection->rollBack();
    }
}
```

---

## 6. Models

Models will encapsulate database operations and business logic related to specific entities. They will utilize the Database class for database interactions.

### 6.1 Base Model

**File: app/models/BaseModel.php**
```php
<?php
namespace App\Models;

use App\Includes\Database;

abstract class BaseModel {
    protected $db;
    protected $table;
    protected $primaryKey = 'id';
    protected $fillable = [];
    
    public function __construct() {
        $this->db = Database::getInstance();
    }
    
    // Find record by primary key
    public function find($id) {
        $this->db->query("SELECT * FROM {$this->table} WHERE {$this->primaryKey} = :id");
        $this->db->bind(':id', $id);
        return $this->db->single();
    }
    
    // Get all records
    public function all() {
        $this->db->query("SELECT * FROM {$this->table}");
        return $this->db->resultSet();
    }
    
    // Create new record
    public function create(array $data) {
        // Filter data to only include fillable fields
        $fields = array_intersect_key($data, array_flip($this->fillable));
        
        if (empty($fields)) {
            return false;
        }
        
        $keys = array_keys($fields);
        $fieldsString = implode(', ', $keys);
        $placeholders = ':' . implode(', :', $keys);
        
        $this->db->query("INSERT INTO {$this->table} ({$fieldsString}) VALUES ({$placeholders})");
        
        foreach ($fields as $key => $value) {
            $this->db->bind(':' . $key, $value);
        }
        
        if ($this->db->execute()) {
            return $this->db->lastInsertId();
        }
        
        return false;
    }
    
    // Update record
    public function update($id, array $data) {
        // Filter data to only include fillable fields
        $fields = array_intersect_key($data, array_flip($this->fillable));
        
        if (empty($fields)) {
            return false;
        }
        
        $fieldsString = '';
        foreach ($fields as $key => $value) {
            $fieldsString .= "{$key} = :{$key}, ";
        }
        $fieldsString = rtrim($fieldsString, ', ');
        
        $this->db->query("UPDATE {$this->table} SET {$fieldsString} WHERE {$this->primaryKey} = :id");
        
        foreach ($fields as $key => $value) {
            $this->db->bind(':' . $key, $value);
        }
        $this->db->bind(':id', $id);
        
        return $this->db->execute();
    }
    
    // Delete record
    public function delete($id) {
        $this->db->query("DELETE FROM {$this->table} WHERE {$this->primaryKey} = :id");
        $this->db->bind(':id', $id);
        return $this->db->execute();
    }
    
    // Count records
    public function count() {
        $this->db->query("SELECT COUNT(*) as count FROM {$this->table}");
        $result = $this->db->single();
        return $result['count'];
    }
    
    // Custom query
    public function query($sql, $params = []) {
        $this->db->query($sql);
        
        foreach ($params as $key => $value) {
            $this->db->bind($key, $value);
        }
        
        return $this->db->resultSet();
    }

    // Single result from custom query
    public function queryOne($sql, $params = []) {
        $this->db->query($sql);
        
        foreach ($params as $key => $value) {
            $this->db->bind($key, $value);
        }
        
        return $this->db->single();
    }
}
```

### 6.2 Product Model

**File: app/models/Product.php**
```php
<?php
namespace App\Models;

class Product extends BaseModel {
    protected $table = 'products';
    protected $fillable = [
        'sku', 'name', 'slug', 'description', 'short_description', 
        'price', 'sale_price', 'cost', 'quantity', 'is_featured',
        'is_active', 'meta_title', 'meta_description', 'category_id', 
        'brand_id', 'tax_class_id'
    ];
    
    // Get all active products
    public function getActiveProducts($limit = null, $offset = 0) {
        $limitSql = $limit ? "LIMIT {$offset}, {$limit}" : '';
        
        $this->db->query("
            SELECT p.*, c.name as category_name, c.slug as category_slug,
                   b.name as brand_name, b.slug as brand_slug,
                   (SELECT image FROM product_images WHERE product_id = p.id ORDER BY sort_order LIMIT 1) as main_image
            FROM {$this->table} p
            LEFT JOIN categories c ON p.category_id = c.id
            LEFT JOIN brands b ON p.brand_id = b.id
            WHERE p.is_active = 1
            ORDER BY p.created_at DESC
            {$limitSql}
        ");
        
        return $this->db->resultSet();
    }
    
    // Get featured products
    public function getFeaturedProducts($limit = 4) {
        $this->db->query("
            SELECT p.*, c.name as category_name, c.slug as category_slug,
                   (SELECT image FROM product_images WHERE product_id = p.id ORDER BY sort_order LIMIT 1) as main_image
            FROM {$this->table} p
            LEFT JOIN categories c ON p.category_id = c.id
            WHERE p.is_active = 1 AND p.is_featured = 1
            ORDER BY p.created_at DESC
            LIMIT :limit
        ");
        
        $this->db->bind(':limit', $limit);
        return $this->db->resultSet();
    }
    
    // Get product by slug
    public function getBySlug($slug) {
        $this->db->query("
            SELECT p.*, c.name as category_name, c.slug as category_slug,
                   b.name as brand_name, b.slug as brand_slug
            FROM {$this->table} p
            LEFT JOIN categories c ON p.category_id = c.id
            LEFT JOIN brands b ON p.brand_id = b.id
            WHERE p.slug = :slug AND p.is_active = 1
        ");
        
        $this->db->bind(':slug', $slug);
        return $this->db->single();
    }
    
    // Get products by category
    public function getByCategory($categoryId, $limit = null, $offset = 0) {
        $limitSql = $limit ? "LIMIT {$offset}, {$limit}" : '';
        
        $this->db->query("
            SELECT p.*, c.name as category_name, c.slug as category_slug,
                   (SELECT image FROM product_images WHERE product_id = p.id ORDER BY sort_order LIMIT 1) as main_image
            FROM {$this->table} p
            LEFT JOIN categories c ON p.category_id = c.id
            WHERE p.category_id = :category_id AND p.is_active = 1
            ORDER BY p.created_at DESC
            {$limitSql}
        ");
        
        $this->db->bind(':category_id', $categoryId);
        return $this->db->resultSet();
    }
    
    // Get product images
    public function getImages($productId) {
        $this->db->query("
            SELECT * FROM product_images 
            WHERE product_id = :product_id 
            ORDER BY sort_order
        ");
        
        $this->db->bind(':product_id', $productId);
        return $this->db->resultSet();
    }
    
    // Get product attributes
    public function getAttributes($productId) {
        $this->db->query("
            SELECT pa.*, a.name as attribute_name, a.display_name
            FROM product_attributes pa
            JOIN attributes a ON pa.attribute_id = a.id
            WHERE pa.product_id = :product_id
            ORDER BY a.sort_order
        ");
        
        $this->db->bind(':product_id', $productId);
        return $this->db->resultSet();
    }
    
    // Get related products
    public function getRelatedProducts($productId, $categoryId, $limit = 4) {
        $this->db->query("
            SELECT p.*, 
                   (SELECT image FROM product_images WHERE product_id = p.id ORDER BY sort_order LIMIT 1) as main_image
            FROM {$this->table} p
            WHERE p.category_id = :category_id 
              AND p.id != :product_id 
              AND p.is_active = 1
            ORDER BY RAND()
            LIMIT :limit
        ");
        
        $this->db->bind(':category_id', $categoryId);
        $this->db->bind(':product_id', $productId);
        $this->db->bind(':limit', $limit);
        return $this->db->resultSet();
    }
    
    // Search products
    public function search($term, $limit = null, $offset = 0) {
        $limitSql = $limit ? "LIMIT {$offset}, {$limit}" : '';
        $searchTerm = "%{$term}%";
        
        $this->db->query("
            SELECT p.*, c.name as category_name, c.slug as category_slug,
                   (SELECT image FROM product_images WHERE product_id = p.id ORDER BY sort_order LIMIT 1) as main_image
            FROM {$this->table} p
            LEFT JOIN categories c ON p.category_id = c.id
            WHERE p.is_active = 1 
              AND (p.name LIKE :term 
                OR p.description LIKE :term 
                OR p.short_description LIKE :term)
            ORDER BY p.created_at DESC
            {$limitSql}
        ");
        
        $this->db->bind(':term', $searchTerm);
        return $this->db->resultSet();
    }
    
    // Update product inventory
    public function updateInventory($productId, $quantity) {
        $this->db->query("
            UPDATE {$this->table} 
            SET quantity = quantity - :quantity 
            WHERE id = :product_id AND quantity >= :quantity
        ");
        
        $this->db->bind(':product_id', $productId);
        $this->db->bind(':quantity', $quantity);
        return $this->db->execute();
    }
}
```

### 6.3 User Model

**File: app/models/User.php**
```php
<?php
namespace App\Models;

class User extends BaseModel {
    protected $table = 'users';
    protected $fillable = [
        'email', 'password', 'first_name', 'last_name', 'phone',
        'role', 'is_active', 'email_verified_at', 'remember_token',
        'reset_token', 'reset_token_expiry', 'last_login'
    ];
    
    // Find user by email
    public function findByEmail($email) {
        $this->db->query("SELECT * FROM {$this->table} WHERE email = :email");
        $this->db->bind(':email', $email);
        return $this->db->single();
    }
    
    // Create new user
    public function register($data) {
        // Hash password
        $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT);
        
        return $this->create($data);
    }
    
    // Validate login
    public function login($email, $password) {
        $user = $this->findByEmail($email);
        
        if (!$user) {
            return false;
        }
        
        if (!$user['is_active']) {
            return false;
        }
        
        if (password_verify($password, $user['password'])) {
            // Update last login time
            $this->update($user['id'], ['last_login' => date('Y-m-d H:i:s')]);
            return $user;
        }
        
        return false;
    }
    
    // Get user addresses
    public function getAddresses($userId) {
        $this->db->query("
            SELECT * FROM user_addresses 
            WHERE user_id = :user_id 
            ORDER BY is_default DESC
        ");
        
        $this->db->bind(':user_id', $userId);
        return $this->db->resultSet();
    }
    
    // Get user orders
    public function getOrders($userId, $limit = null, $offset = 0) {
        $limitSql = $limit ? "LIMIT {$offset}, {$limit}" : '';
        
        $this->db->query("
            SELECT o.*, os.name as status_name
            FROM orders o
            JOIN order_statuses os ON o.status_id = os.id
            WHERE o.user_id = :user_id
            ORDER BY o.created_at DESC
            {$limitSql}
        ");
        
        $this->db->bind(':user_id', $userId);
        return $this->db->resultSet();
    }
    
    // Generate password reset token
    public function generateResetToken($email) {
        $user = $this->findByEmail($email);
        
        if (!$user) {
            return false;
        }
        
        $token = bin2hex(random_bytes(32));
        $expiry = date('Y-m-d H:i:s', strtotime('+1 hour'));
        
        $this->update($user['id'], [
            'reset_token' => $token,
            'reset_token_expiry' => $expiry
        ]);
        
        return $token;
    }
    
    // Verify reset token
    public function verifyResetToken($token) {
        $this->db->query("
            SELECT * FROM {$this->table} 
            WHERE reset_token = :token 
              AND reset_token_expiry > NOW()
        ");
        
        $this->db->bind(':token', $token);
        return $this->db->single();
    }
    
    // Reset password
    public function resetPassword($token, $password) {
        $user = $this->verifyResetToken($token);
        
        if (!$user) {
            return false;
        }
        
        $hashedPassword = password_hash($password, PASSWORD_DEFAULT);
        
        $this->update($user['id'], [
            'password' => $hashedPassword,
            'reset_token' => null,
            'reset_token_expiry' => null
        ]);
        
        return true;
    }
}
```

---

## 7. Controllers

Controllers will handle incoming requests, interact with models, and pass data to views.

### 7.1 Base Controller

**File: app/controllers/BaseController.php**
```php
<?php
namespace App\Controllers;

class BaseController {
    // Render a view
    protected function view($view, $data = []) {
        // Extract data to make variables available in view
        extract($data);
        
        // Start output buffering
        ob_start();
        
        // Include the view file
        $viewPath = APP_ROOT . "/views/{$view}.php";
        if (file_exists($viewPath)) {
            require_once $viewPath;
        } else {
            die("View {$view} not found");
        }
        
        // Get the content of the buffer
        $content = ob_get_clean();
        
        // Return the content
        return $content;
    }
    
    // Load a model
    protected function model($model) {
        $modelClass = "\\App\\Models\\{$model}";
        return new $modelClass();
    }
    
    // Redirect to another page
    protected function redirect($url) {
        header("Location: {$url}");
        exit;
    }
    
    // Return JSON response
    protected function json($data, $statusCode = 200) {
        header('Content-Type: application/json');
        http_response_code($statusCode);
        echo json_encode($data);
        exit;
    }
    
    // Get request method
    protected function getMethod() {
        return $_SERVER['REQUEST_METHOD'];
    }
    
    // Check if request is POST
    protected function isPost() {
        return $this->getMethod() === 'POST';
    }
    
    // Check if request is GET
    protected function isGet() {
        return $this->getMethod() === 'GET';
    }
    
    // Get POST data
    protected function getPost($key = null, $default = null) {
        if ($key === null) {
            return $_POST;
        }
        
        return isset($_POST[$key]) ? $_POST[$key] : $default;
    }
    
    // Get GET data
    protected function getQuery($key = null, $default = null) {
        if ($key === null) {
            return $_GET;
        }
        
        return isset($_GET[$key]) ? $_GET[$key] : $default;
    }
    
    // Get JSON data from request body
    protected function getJsonInput() {
        $json = file_get_contents('php://input');
        return json_decode($json, true);
    }
    
    // Validate CSRF token
    protected function validateToken() {
        $token = $this->getPost('csrf_token');
        if (!$token || $token !== $_SESSION['csrf_token']) {
            // Log potential CSRF attack
            error_log('CSRF token validation failed');
            return false;
        }
        return true;
    }
    
    // Generate CSRF token
    protected function generateToken() {
        if (!isset($_SESSION['csrf_token'])) {
            $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
        }
        return $_SESSION['csrf_token'];
    }
}
```

### 7.2 Home Controller

**File: app/controllers/HomeController.php**
```php
<?php
namespace App\Controllers;

use App\Models\Product;
use App\Models\Category;

class HomeController extends BaseController {
    protected $productModel;
    protected $categoryModel;
    
    public function __construct() {
        $this->productModel = new Product();
        $this->categoryModel = new Category();
    }
    
    // Display homepage
    public function index() {
        // Get featured products
        $featuredProducts = $this->productModel->getFeaturedProducts(8);
        
        // Get main categories
        $categories = $this->categoryModel->getMainCategories();
        
        // Pass data to view
        $data = [
            'title' => 'LUMIÈRE - Luxury Aromatherapy Products',
            'featuredProducts' => $featuredProducts,
            'categories' => $categories,
            'csrf_token' => $this->generateToken()
        ];
        
        // Render view with data
        return $this->view('home/index', $data);
    }
    
    // Display about page
    public function about() {
        $data = [
            'title' => 'About Us | LUMIÈRE',
            'csrf_token' => $this->generateToken()
        ];
        
        return $this->view('home/about', $data);
    }
    
    // Display contact page
    public function contact() {
        $data = [
            'title' => 'Contact Us | LUMIÈRE',
            'csrf_token' => $this->generateToken()
        ];
        
        return $this->view('home/contact', $data);
    }
    
    // Process contact form submission
    public function submitContact() {
        if (!$this->isPost()) {
            $this->redirect('/contact');
        }
        
        if (!$this->validateToken()) {
            $this->redirect('/contact?error=invalid_token');
        }
        
        // Validate form data
        $name = trim($this->getPost('name'));
        $email = trim($this->getPost('email'));
        $subject = trim($this->getPost('subject'));
        $message = trim($this->getPost('message'));
        
        $errors = [];
        
        if (empty($name)) {
            $errors['name'] = 'Name is required';
        }
        
        if (empty($email) || !filter_var($email, FILTER_VALIDATE_EMAIL)) {
            $errors['email'] = 'Valid email is required';
        }
        
        if (empty($subject)) {
            $errors['subject'] = 'Subject is required';
        }
        
        if (empty($message)) {
            $errors['message'] = 'Message is required';
        }
        
        if (!empty($errors)) {
            // Store errors and form data in session
            $_SESSION['contact_errors'] = $errors;
            $_SESSION['contact_data'] = [
                'name' => $name,
                'email' => $email,
                'subject' => $subject,
                'message' => $message
            ];
            
            $this->redirect('/contact');
        }
        
        // Process form (send email, store in database, etc.)
        // Here we would typically use the EmailService to send the contact form
        
        // Set success message
        $_SESSION['success_message'] = 'Your message has been sent. We will get back to you soon!';
        
        $this->redirect('/contact');
    }
}
```

### 7.3 Product Controller

**File: app/controllers/ProductController.php**
```php
<?php
namespace App\Controllers;

use App\Models\Product;
use App\Models\Category;
use App\Models\Review;

class ProductController extends BaseController {
    protected $productModel;
    protected $categoryModel;
    protected $reviewModel;
    
    public function __construct() {
        $this->productModel = new Product();
        $this->categoryModel = new Category();
        $this->reviewModel = new Review();
    }
    
    // Display all products
    public function index() {
        // Get pagination parameters
        $page = $this->getQuery('page', 1);
        $perPage = 12;
        $offset = ($page - 1) * $perPage;
        
        // Get filter parameters
        $categorySlug = $this->getQuery('category');
        $sortBy = $this->getQuery('sort', 'newest');
        $priceMin = $this->getQuery('price_min');
        $priceMax = $this->getQuery('price_max');
        
        // Get category ID from slug if provided
        $categoryId = null;
        if ($categorySlug) {
            $category = $this->categoryModel->getBySlug($categorySlug);
            $categoryId = $category ? $category['id'] : null;
        }
        
        // Get filtered products
        $products = $this->productModel->getFiltered(
            $categoryId,
            $sortBy,
            $priceMin,
            $priceMax,
            $perPage,
            $offset
        );
        
        // Get total count for pagination
        $totalProducts = $this->productModel->countFiltered(
            $categoryId,
            $priceMin,
            $priceMax
        );
        
        // Calculate total pages
        $totalPages = ceil($totalProducts / $perPage);
        
        // Get all categories for filter
        $categories = $this->categoryModel->getAll();
        
        $data = [
            'title' => 'Shop | LUMIÈRE',
            'products' => $products,
            'categories' => $categories,
            'currentCategory' => $categorySlug,
            'currentSort' => $sortBy,
            'priceMin' => $priceMin,
            'priceMax' => $priceMax,
            'pagination' => [
                'current' => $page,
                'total' => $totalPages,
                'perPage' => $perPage,
                'totalItems' => $totalProducts
            ],
            'csrf_token' => $this->generateToken()
        ];
        
        return $this->view('products/listing', $data);
    }
    
    // Display single product
    public function show($slug) {
        // Get product by slug
        $product = $this->productModel->getBySlug($slug);
        
        if (!$product) {
            $this->redirect('/products?error=product_not_found');
        }
        
        // Get product images
        $images = $this->productModel->getImages($product['id']);
        
        // Get product attributes
        $attributes = $this->productModel->getAttributes($product['id']);
        
        // Get related products
        $relatedProducts = $this->productModel->getRelatedProducts(
            $product['id'],
            $product['category_id'],
            4
        );
        
        // Get product reviews
        $reviews = $this->reviewModel->getByProductId($product['id']);
        
        $data = [
            'title' => $product['name'] . ' | LUMIÈRE',
            'product' => $product,
            'images' => $images,
            'attributes' => $attributes,
            'relatedProducts' => $relatedProducts,
            'reviews' => $reviews,
            'csrf_token' => $this->generateToken()
        ];
        
        return $this->view('products/detail', $data);
    }
    
    // Submit product review
    public function submitReview($productId) {
        if (!$this->isPost()) {
            $this->redirect("/products/{$productId}");
        }
        
        if (!$this->validateToken()) {
            $this->redirect("/products/{$productId}?error=invalid_token");
        }
        
        // Check if user is logged in
        if (!isset($_SESSION['user_id'])) {
            $this->redirect("/login?redirect=/products/{$productId}");
        }
        
        // Validate form data
        $rating = (int) $this->getPost('rating');
        $title = trim($this->getPost('title'));
        $comment = trim($this->getPost('comment'));
        
        $errors = [];
        
        if ($rating < 1 || $rating > 5) {
            $errors['rating'] = 'Rating must be between 1 and 5';
        }
        
        if (empty($title)) {
            $errors['title'] = 'Title is required';
        }
        
        if (empty($comment)) {
            $errors['comment'] = 'Comment is required';
        }
        
        if (!empty($errors)) {
            $_SESSION['review_errors'] = $errors;
            $_SESSION['review_data'] = [
                'rating' => $rating,
                'title' => $title,
                'comment' => $comment
            ];
            
            $this->redirect("/products/{$productId}#review-form");
        }
        
        // Create review
        $reviewData = [
            'product_id' => $productId,
            'user_id' => $_SESSION['user_id'],
            'rating' => $rating,
            'title' => $title,
            'comment' => $comment
        ];
        
        $reviewId = $this->reviewModel->create($reviewData);
        
        if (!$reviewId) {
            $_SESSION['error_message'] = 'Failed to submit review. Please try again.';
            $this->redirect("/products/{$productId}#review-form");
        }
        
        $_SESSION['success_message'] = 'Your review has been submitted successfully!';
        $this->redirect("/products/{$productId}#reviews");
    }
    
    // Search products
    public function search() {
        $query = $this->getQuery('q', '');
        
        if (empty($query)) {
            $this->redirect('/products');
        }
        
        // Get pagination parameters
        $page = $this->getQuery('page', 1);
        $perPage = 12;
        $offset = ($page - 1) * $perPage;
        
        // Search products
        $products = $this->productModel->search($query, $perPage, $offset);
        
        // Count total results
        $totalProducts = $this->productModel->countSearch($query);
        
        // Calculate total pages
        $totalPages = ceil($totalProducts / $perPage);
        
        $data = [
            'title' => 'Search Results for "' . htmlspecialchars($query) . '" | LUMIÈRE',
            'products' => $products,
            'searchQuery' => $query,
            'pagination' => [
                'current' => $page,
                'total' => $totalPages,
                'perPage' => $perPage,
                'totalItems' => $totalProducts
            ],
            'csrf_token' => $this->generateToken()
        ];
        
        return $this->view('products/search', $data);
    }
}
```

---

## 8. Views

Views are responsible for rendering HTML and displaying data to the user. They will be organized in a hierarchical structure to promote reusability.

### 8.1 Layout Templates

**File: app/views/layout/master.php**
```php
<!DOCTYPE html>
<html lang="en" data-theme="<?php echo isset($_SESSION['theme']) ? $_SESSION['theme'] : 'day'; ?>">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="<?php echo isset($metaDescription) ? $metaDescription : 'LUMIÈRE | Luxury natural beauty and aromatherapy essences crafted with the finest botanical ingredients.'; ?>">
    <title><?php echo isset($title) ? $title : 'LUMIÈRE | Luxury Natural Beauty & Aromatherapy'; ?></title>
    
    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500&family=Cinzel:wght@400;500;600&family=Jost:wght@200;300;400;500&family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400;1,500&display=swap" rel="stylesheet">
    
    <!-- Favicon -->
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>✨</text></svg>">
    
    <!-- Styles -->
    <link rel="stylesheet" href="/css/main.css">
    <?php if (isset($extraStyles)): ?>
        <?php foreach ($extraStyles as $style): ?>
            <link rel="stylesheet" href="<?php echo $style; ?>">
        <?php endforeach; ?>
    <?php endif; ?>
    
    <!-- CSRF Token -->
    <meta name="csrf-token" content="<?php echo $csrf_token; ?>">
</head>
<body>
    <!-- Custom cursor -->
    <div class="custom-cursor"></div>
    <div class="custom-cursor-inner"></div>
    
    <!-- Particle background -->
    <div class="particles-container" id="particles-js"></div>
    
    <!-- Header -->
    <?php include(APP_ROOT . '/views/partials/header.php'); ?>
    
    <!-- Mobile Navigation -->
    <?php include(APP_ROOT . '/views/partials/mobile-nav.php'); ?>
    
    <!-- Main Content -->
    <main>
        <?php echo $content; ?>
    </main>
    
    <!-- Footer -->
    <?php include(APP_ROOT . '/views/partials/footer.php'); ?>
    
    <!-- Scripts -->
    <script src="https://cdn.jsdelivr.net/npm/particles.js@2.0.0/particles.min.js"></script>
    <script src="/js/main.js"></script>
    <?php if (isset($extraScripts)): ?>
        <?php foreach ($extraScripts as $script): ?>
            <script src="<?php echo $script; ?>"></script>
        <?php endforeach; ?>
    <?php endif; ?>
</body>
</html>
```

**File: app/views/layout/admin.php**
```php
<!DOCTYPE html>
<html lang="en" data-theme="<?php echo isset($_SESSION['theme']) ? $_SESSION['theme'] : 'day'; ?>">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?php echo isset($title) ? $title : 'Admin Dashboard | LUMIÈRE'; ?></title>
    
    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;500;600&family=Jost:wght@300;400;500&display=swap" rel="stylesheet">
    
    <!-- Favicon -->
    <link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>✨</text></svg>">
    
    <!-- Styles -->
    <link rel="stylesheet" href="/css/admin.css">
    <?php if (isset($extraStyles)): ?>
        <?php foreach ($extraStyles as $style): ?>
            <link rel="stylesheet" href="<?php echo $style; ?>">
        <?php endforeach; ?>
    <?php endif; ?>
    
    <!-- CSRF Token -->
    <meta name="csrf-token" content="<?php echo $csrf_token; ?>">
</head>
<body class="admin-body">
    <!-- Admin Header -->
    <?php include(APP_ROOT . '/views/admin/partials/header.php'); ?>
    
    <div class="admin-container">
        <!-- Admin Sidebar -->
        <?php include(APP_ROOT . '/views/admin/partials/sidebar.php'); ?>
        
        <!-- Main Content -->
        <main class="admin-content">
            <?php if (isset($_SESSION['success_message'])): ?>
                <div class="alert alert-success">
                    <?php echo $_SESSION['success_message']; ?>
                    <?php unset($_SESSION['success_message']); ?>
                </div>
            <?php endif; ?>
            
            <?php if (isset($_SESSION['error_message'])): ?>
                <div class="alert alert-danger">
                    <?php echo $_SESSION['error_message']; ?>
                    <?php unset($_SESSION['error_message']); ?>
                </div>
            <?php endif; ?>
            
            <?php echo $content; ?>
        </main>
    </div>
    
    <!-- Scripts -->
    <script src="/js/admin.js"></script>
    <?php if (isset($extraScripts)): ?>
        <?php foreach ($extraScripts as $script): ?>
            <script src="<?php echo $script; ?>"></script>
        <?php endforeach; ?>
    <?php endif; ?>
</body>
</html>
```

### 8.2 Home Page View

**File: app/views/home/index.php**
```php
<?php
// Extend master layout
ob_start();
?>

<!-- Hero Section -->
<section class="hero">
    <div class="hero-media">
        <video autoplay muted loop class="hero-video">
            <source src="/videos/hero-background.mp4" type="video/mp4">
            <!-- Fallback image -->
            <img src="/images/banners/hero-fallback.jpg" alt="Luxury aromatherapy products" class="hero-image">
        </video>
    </div>
    <div class="hero-overlay"></div>
    <div class="container">
        <div class="hero-content">
            <div class="subtitle">NATURE'S FINEST ESSENCE</div>
            <h1 class="hero-title">Elevate Your Senses <span>with Botanical Luxury</span></h1>
            <p class="hero-subtitle">Exquisite aromatherapy and skin rituals crafted from the world's most precious natural ingredients, designed to transform your daily routine into moments of sublime indulgence.</p>
            <div class="hero-actions">
                <a href="/products" class="btn btn-primary btn-lg">Explore Collections</a>
                <a href="/about" class="btn btn-lg">Discover Our Story</a>
            </div>
        </div>
    </div>
    <div class="hero-scroll" id="scroll-down">
        <div class="hero-scroll-text">Scroll to discover</div>
        <div class="hero-scroll-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="12" y1="5" x2="12" y2="19"></line>
                <polyline points="19 12 12 19 5 12"></polyline>
            </svg>
        </div>
    </div>
</section>

<!-- Featured Collection Section -->
<section class="section featured-collection">
    <div class="container-xl">
        <div class="collection-header fade-in">
            <div class="subtitle">CURATED SELECTION</div>
            <h2>Experience Our Signature Collections</h2>
            <p>Discover our most coveted formulations, each crafted to deliver exceptional sensory experiences</p>
        </div>
        
        <div class="featured-grid">
            <?php foreach ($categories as $index => $category): ?>
                <?php if ($index < 4): ?>
                    <div class="featured-item featured-item-<?php echo $index + 1; ?> fade-in" <?php if ($index > 0): ?>style="animation-delay: <?php echo $index * 0.1; ?>s;"<?php endif; ?>>
                        <img src="<?php echo $category['image']; ?>" alt="<?php echo $category['name']; ?>" class="featured-image">
                        <div class="featured-content">
                            <div class="featured-subtitle"><?php echo $category['subtitle']; ?></div>
                            <h3 class="featured-title"><?php echo $category['name']; ?></h3>
                            <a href="/products?category=<?php echo $category['slug']; ?>" class="featured-link">Explore Collection <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="5" y1="12" x2="19" y2="12"></line><polyline points="12 5 19 12 12 19"></polyline></svg></a>
                        </div>
                    </div>
                <?php endif; ?>
            <?php endforeach; ?>
        </div>
        
        <div class="featured-cta fade-in">
            <a href="/products" class="btn btn-primary">View All Collections</a>
        </div>
    </div>
</section>

<!-- Product Collections Section -->
<section class="section collections">
    <div class="container">
        <div class="section-header fade-in">
            <div class="subtitle">BESTSELLERS</div>
            <h2>Most Coveted Formulations</h2>
            <p>Discover our most beloved creations that have earned a devoted following worldwide</p>
        </div>
        
        <div class="products-grid stagger-fade-in">
            <?php foreach ($featuredProducts as $product): ?>
                <div class="product-card">
                    <div class="product-image-wrapper">
                        <img src="<?php echo $product['main_image']; ?>" alt="<?php echo $product['name']; ?>" class="product-image">
                        <?php if ($product['is_featured']): ?>
                            <div class="product-badge">Bestseller</div>
                        <?php endif; ?>
                        <div class="product-actions">
                            <a href="/products/<?php echo $product['slug']; ?>" class="product-action" aria-label="Quick view">
                                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                    <circle cx="11" cy="11" r="8"></circle>
                                    <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
                                    <line x1="11" y1="8" x2="11" y2="14"></line>
                                    <line x1="8" y1="11" x2="14" y2="11"></line>
                                </svg>
                            </a>
                            <button class="product-action add-to-wishlist" data-product-id="<?php echo $product['id']; ?>" aria-label="Add to wishlist">
                                <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                    <path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"></path>
                                </svg>
                            </button>
                        </div>
                    </div>
                    <div class="product-content">
                        <div class="product-category"><?php echo $product['category_name']; ?></div>
                        <h3 class="product-title"><?php echo $product['name']; ?></h3>
                        <p class="product-description"><?php echo $product['short_description']; ?></p>
                        <div class="product-meta">
                            <div class="product-price">
                                <?php if ($product['sale_price']): ?>
                                    <span class="price-old">$<?php echo number_format($product['price'], 2); ?></span>
                                    <span class="price-current">$<?php echo number_format($product['sale_price'], 2); ?></span>
                                <?php else: ?>
                                    <span class="price-current">$<?php echo number_format($product['price'], 2); ?></span>
                                <?php endif; ?>
                            </div>
                        </div>
                        <div class="product-cta">
                            <button class="btn btn-outline btn-block add-to-cart" data-product-id="<?php echo $product['id']; ?>">Add to Cart</button>
                        </div>
                    </div>
                </div>
            <?php endforeach; ?>
        </div>
        
        <div class="view-all fade-in">
            <a href="/products" class="btn btn-primary">View All Products</a>
        </div>
    </div>
</section>

<!-- Brand Story Section -->
<section class="section-lg brand-story">
    <div class="brand-story-bg" style="background-image: url('/images/banners/brand-story-bg.jpg');"></div>
    <div class="container">
        <div class="brand-story-inner">
            <div class="brand-story-media fade-in-left">
                <img src="/images/banners/brand-story.jpg" alt="Handcrafted natural products" class="brand-story-image">
            </div>
            
            <div class="story-content fade-in-right">
                <div class="story-header">
                    <div class="subtitle">OUR STORY</div>
                    <h2 class="story-title">Crafted with Intention, Sourced with Integrity</h2>
                </div>
                
                <p class="story-text drop-cap">At LUMIÈRE, we believe in the transformative power of nature's most precious botanicals. Our journey began with a quest to create extraordinary beauty and wellness products that honor ancient traditions while embracing modern science, creating sensory experiences that elevate the everyday.</p>
                
                <p class="story-text">Each formulation is meticulously crafted in small batches by our master artisans, ensuring uncompromising quality and effectiveness. We travel the world to source the finest ingredients directly from sustainable farms and ethical producers who share our commitment to both people and planet.</p>
                
                <div class="story-features">
                    <div class="story-feature">
                        <div class="feature-icon">
                            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path>
                            </svg>
                        </div>
                        <div class="feature-content">
                            <h3 class="feature-title">Ethical Sourcing</h3>
                            <p class="feature-text">We partner directly with sustainable farms and artisan cooperatives around the world, ensuring fair compensation and ethical practices.</p>
                        </div>
                    </div>
                    
                    <div class="story-feature">
                        <div class="feature-icon">
                            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                <path d="M21 16V8a2 2 0 0 0-1-1.73l-7-4a2 2 0 0 0-2 0l-7 4A2 2 0 0 0 3 8v8a2 2 0 0 0 1 1.73l7 4a2 2 0 0 0 2 0l7-4A2 2 0 0 0 21 16z"></path>
                                <polyline points="3.27 6.96 12 12.01 20.73 6.96"></polyline>
                                <line x1="12" y1="22.08" x2="12" y2="12"></line>
                            </svg>
                        </div>
                        <div class="feature-content">
                            <h3 class="feature-title">Artisanal Craftsmanship</h3>
                            <p class="feature-text">Each product is handcrafted in our atelier using traditional methods that preserve the integrity of our precious ingredients.</p>
                        </div>
                    </div>
                </div>
                
                <a href="/about" class="btn btn-outline">Learn More About Our Journey</a>
            </div>
        </div>
    </div>
</section>

<!-- Newsletter Section -->
<section class="newsletter">
    <div class="newsletter-circles">
        <div class="newsletter-circle newsletter-circle-1"></div>
        <div class="newsletter-circle newsletter-circle-2"></div>
    </div>
    <div class="container">
        <div class="newsletter-inner">
            <div class="newsletter-content fade-in">
                <h2 class="newsletter-title">Join Our Inner Circle</h2>
                <p class="newsletter-subtitle">Subscribe to receive exclusive offers, early access to new releases, and curated wellness rituals from our master formulators.</p>
            </div>
            
            <form class="newsletter-form fade-in" action="/subscribe" method="post">
                <input type="hidden" name="csrf_token" value="<?php echo $csrf_token; ?>">
                <input type="email" name="email" class="newsletter-input" placeholder="Your email address" required>
                <button type="submit" class="newsletter-submit">Subscribe</button>
            </form>
        </div>
    </div>
</section>

<?php
$content = ob_get_clean();

// Define extra scripts and styles
$extraScripts = ['/js/home.js'];
$extraStyles = [];

// Include master layout
include(APP_ROOT . '/views/layout/master.php');
```

---

## 9. Security Implementation

### 9.1 Authentication Service

**File: app/services/AuthService.php**
```php
<?php
namespace App\Services;

use App\Models\User;

class AuthService {
    protected $userModel;
    
    public function __construct() {
        $this->userModel = new User();
    }
    
    // Authenticate user
    public function authenticate($email, $password) {
        $user = $this->userModel->login($email, $password);
        
        if (!$user) {
            return false;
        }
        
        // Set session data
        $_SESSION['user_id'] = $user['id'];
        $_SESSION['user_email'] = $user['email'];
        $_SESSION['user_name'] = $user['first_name'] . ' ' . $user['last_name'];
        $_SESSION['user_role'] = $user['role'];
        
        // Set remember cookie if requested
        if (isset($_POST['remember']) && $_POST['remember'] == 1) {
            $token = bin2hex(random_bytes(32));
            $expiry = time() + (30 * 24 * 60 * 60); // 30 days
            
            // Save token in database
            $this->userModel->update($user['id'], ['remember_token' => $token]);
            
            // Set cookie
            setcookie('remember_token', $token, $expiry, '/', '', true, true);
        }
        
        return true;
    }
    
    // Check if user is logged in
    public function isLoggedIn() {
        return isset($_SESSION['user_id']);
    }
    
    // Check if user is admin
    public function isAdmin() {
        return $this->isLoggedIn() && $_SESSION['user_role'] === 'admin';
    }
    
    // Check remember token
    public function checkRememberToken() {
        if (!isset($_COOKIE['remember_token'])) {
            return false;
        }
        
        $token = $_COOKIE['remember_token'];
        
        // Find user by token
        $user = $this->userModel->findByRememberToken($token);
        
        if (!$user) {
            // Invalid token, clear cookie
            setcookie('remember_token', '', time() - 3600, '/', '', true, true);
            return false;
        }
        
        // Set session data
        $_SESSION['user_id'] = $user['id'];
        $_SESSION['user_email'] = $user['email'];
        $_SESSION['user_name'] = $user['first_name'] . ' ' . $user['last_name'];
        $_SESSION['user_role'] = $user['role'];
        
        return true;
    }
    
    // Logout user
    public function logout() {
        // Clear session
        unset($_SESSION['user_id']);
        unset($_SESSION['user_email']);
        unset($_SESSION['user_name']);
        unset($_SESSION['user_role']);
        
        // Clear remember token cookie
        if (isset($_COOKIE['remember_token'])) {
            setcookie('remember_token', '', time() - 3600, '/', '', true, true);
        }
        
        // Regenerate session ID
        session_regenerate_id(true);
    }
    
    // Generate password reset token
    public function generateResetToken($email) {
        return $this->userModel->generateResetToken($email);
    }
    
    // Verify reset token
    public function verifyResetToken($token) {
        return $this->userModel->verifyResetToken($token);
    }
    
    // Reset password
    public function resetPassword($token, $password) {
        return $this->userModel->resetPassword($token, $password);
    }
    
    // Register new user
    public function register($data) {
        // Validate data
        if (empty($data['email']) || !filter_var($data['email'], FILTER_VALIDATE_EMAIL)) {
            return ['success' => false, 'message' => 'Valid email is required'];
        }
        
        if (empty($data['password']) || strlen($data['password']) < 8) {
            return ['success' => false, 'message' => 'Password must be at least 8 characters long'];
        }
        
        if (empty($data['first_name']) || empty($data['last_name'])) {
            return ['success' => false, 'message' => 'First name and last name are required'];
        }
        
        // Check if email already exists
        $existingUser = $this->userModel->findByEmail($data['email']);
        if ($existingUser) {
            return ['success' => false, 'message' => 'Email already in use'];
        }
        
        // Register user
        $userId = $this->userModel->register($data);
        
        if (!$userId) {
            return ['success' => false, 'message' => 'Registration failed. Please try again.'];
        }
        
        return ['success' => true, 'user_id' => $userId];
    }
}
```

### 9.2 CSRF Protection Middleware

**File: app/middleware/CSRF.php**
```php
<?php
namespace App\Middleware;

class CSRF {
    // Generate CSRF token
    public static function generateToken() {
        if (!isset($_SESSION['csrf_token'])) {
            $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
        }
        return $_SESSION['csrf_token'];
    }
    
    // Validate CSRF token
    public static function validateToken($token) {
        if (!isset($_SESSION['csrf_token']) || $token !== $_SESSION['csrf_token']) {
            return false;
        }
        return true;
    }
    
    // Middleware handler
    public static function handle() {
        // Skip CSRF check for GET requests
        if ($_SERVER['REQUEST_METHOD'] === 'GET') {
            return true;
        }
        
        // Check for token in POST data or header
        $token = $_POST['csrf_token'] ?? $_SERVER['HTTP_X_CSRF_TOKEN'] ?? null;
        
        if (!$token || !self::validateToken($token)) {
            // Log potential CSRF attack
            error_log('CSRF token validation failed');
            
            // Return 403 Forbidden
            http_response_code(403);
            echo json_encode(['error' => 'CSRF token validation failed']);
            exit;
        }
        
        return true;
    }
}
```

### 9.3 Authentication Middleware

**File: app/middleware/Authenticate.php**
```php
<?php
namespace App\Middleware;

use App\Services\AuthService;

class Authenticate {
    // Middleware handler
    public static function handle() {
        $auth = new AuthService();
        
        // Check if user is logged in
        if (!$auth->isLoggedIn()) {
            // Try to authenticate using remember token
            if (!$auth->checkRememberToken()) {
                // Redirect to login page
                header('Location: /login?redirect=' . urlencode($_SERVER['REQUEST_URI']));
                exit;
            }
        }
        
        return true;
    }
}
```

### 9.4 Admin Authentication Middleware

**File: app/middleware/AdminAuth.php**
```php
<?php
namespace App\Middleware;

use App\Services\AuthService;

class AdminAuth {
    // Middleware handler
    public static function handle() {
        $auth = new AuthService();
        
        // Check if user is logged in and is admin
        if (!$auth->isLoggedIn() || !$auth->isAdmin()) {
            // Redirect to login page or access denied page
            if (!$auth->isLoggedIn()) {
                header('Location: /login?redirect=' . urlencode($_SERVER['REQUEST_URI']));
            } else {
                header('Location: /access-denied');
            }
            exit;
        }
        
        return true;
    }
}
```

---

## 10. Routing System

**File: app/routes.php**
```php
<?php
// Define routes
$routes = [
    // Home routes
    '/' => ['controller' => 'HomeController', 'action' => 'index'],
    '/about' => ['controller' => 'HomeController', 'action' => 'about'],
    '/contact' => ['controller' => 'HomeController', 'action' => 'contact'],
    '/contact/submit' => ['controller' => 'HomeController', 'action' => 'submitContact', 'method' => 'POST'],
    
    // Authentication routes
    '/login' => ['controller' => 'AuthController', 'action' => 'login'],
    '/login/process' => ['controller' => 'AuthController', 'action' => 'processLogin', 'method' => 'POST'],
    '/register' => ['controller' => 'AuthController', 'action' => 'register'],
    '/register/process' => ['controller' => 'AuthController', 'action' => 'processRegister', 'method' => 'POST'],
    '/logout' => ['controller' => 'AuthController', 'action' => 'logout'],
    '/forgot-password' => ['controller' => 'AuthController', 'action' => 'forgotPassword'],
    '/reset-password' => ['controller' => 'AuthController', 'action' => 'resetPassword'],
    
    // Product routes
    '/products' => ['controller' => 'ProductController', 'action' => 'index'],
    '/products/search' => ['controller' => 'ProductController', 'action' => 'search'],
    '/products/{slug}' => ['controller' => 'ProductController', 'action' => 'show'],
    '/products/{id}/review' => ['controller' => 'ProductController', 'action' => 'submitReview', 'method' => 'POST', 'middleware' => ['Authenticate']],
    
    // Cart routes
    '/cart' => ['controller' => 'CartController', 'action' => 'index'],
    '/cart/add' => ['controller' => 'CartController', 'action' => 'add', 'method' => 'POST'],
    '/cart/update' => ['controller' => 'CartController', 'action' => 'update', 'method' => 'POST'],
    '/cart/remove' => ['controller' => 'CartController', 'action' => 'remove', 'method' => 'POST'],
    
    // Checkout routes
    '/checkout' => ['controller' => 'CheckoutController', 'action' => 'index', 'middleware' => ['Authenticate']],
    '/checkout/address' => ['controller' => 'CheckoutController', 'action' => 'address', 'middleware' => ['Authenticate']],
    '/checkout/shipping' => ['controller' => 'CheckoutController', 'action' => 'shipping', 'middleware' => ['Authenticate']],
    '/checkout/payment' => ['controller' => 'CheckoutController', 'action' => 'payment', 'middleware' => ['Authenticate']],
    '/checkout/review' => ['controller' => 'CheckoutController', 'action' => 'review', 'middleware' => ['Authenticate']],
    '/checkout/complete' => ['controller' => 'CheckoutController', 'action' => 'complete', 'middleware' => ['Authenticate']],
    
    // User account routes
    '/account' => ['controller' => 'UserController', 'action' => 'index', 'middleware' => ['Authenticate']],
    '/account/profile' => ['controller' => 'UserController', 'action' => 'profile', 'middleware' => ['Authenticate']],
    '/account/orders' => ['controller' => 'UserController', 'action' => 'orders', 'middleware' => ['Authenticate']],
    '/account/orders/{id}' => ['controller' => 'UserController', 'action' => 'orderDetail', 'middleware' => ['Authenticate']],
    '/account/addresses' => ['controller' => 'UserController', 'action' => 'addresses', 'middleware' => ['Authenticate']],
    '/account/wishlist' => ['controller' => 'UserController', 'action' => 'wishlist', 'middleware' => ['Authenticate']],
    
    // Admin routes
    '/admin' => ['controller' => 'AdminController', 'action' => 'index', 'middleware' => ['AdminAuth']],
    '/admin/products' => ['controller' => 'AdminProductController', 'action' => 'index', 'middleware' => ['AdminAuth']],
    '/admin/products/create' => ['controller' => 'AdminProductController', 'action' => 'create', 'middleware' => ['AdminAuth']],
    '/admin/products/edit/{id}' => ['controller' => 'AdminProductController', 'action' => 'edit', 'middleware' => ['AdminAuth']],
    '/admin/orders' => ['controller' => 'AdminOrderController', 'action' => 'index', 'middleware' => ['AdminAuth']],
    '/admin/orders/{id}' => ['controller' => 'AdminOrderController', 'action' => 'show', 'middleware' => ['AdminAuth']],
    '/admin/users' => ['controller' => 'AdminUserController', 'action' => 'index', 'middleware' => ['AdminAuth']],
    
    // API routes
    '/api/products' => ['controller' => 'Api\\ProductController', 'action' => 'index'],
    '/api/cart' => ['controller' => 'Api\\CartController', 'action' => 'index'],
    '/api/cart/add' => ['controller' => 'Api\\CartController', 'action' => 'add', 'method' => 'POST'],
    
    // Misc routes
    '/subscribe' => ['controller' => 'SubscriptionController', 'action' => 'subscribe', 'method' => 'POST'],
    '/404' => ['controller' => 'ErrorController', 'action' => 'notFound'],
];

// Register routes with the application
$app->registerRoutes($routes);
```

**File: app/core/Router.php**
```php
<?php
namespace App\Core;

class Router {
    protected $routes = [];
    
    // Register routes
    public function register($routes) {
        $this->routes = $routes;
    }
    
    // Match route
    public function match($uri, $method = 'GET') {
        // Remove query string
        if (($pos = strpos($uri, '?')) !== false) {
            $uri = substr($uri, 0, $pos);
        }
        
        // Remove trailing slash
        $uri = rtrim($uri, '/');
        
        // If URI is empty, use root route
        if (empty($uri)) {
            $uri = '/';
        }
        
        // Check for exact match
        if (isset($this->routes[$uri])) {
            $route = $this->routes[$uri];
            
            // Check method
            if (isset($route['method']) && $route['method'] !== $method) {
                return false;
            }
            
            return $route;
        }
        
        // Check for dynamic routes
        foreach ($this->routes as $pattern => $route) {
            // Check method
            if (isset($route['method']) && $route['method'] !== $method) {
                continue;
            }
            
            // Convert route pattern to regex
            if (strpos($pattern, '{') !== false) {
                $patternRegex = preg_replace('/{([a-zA-Z0-9_]+)}/', '([^/]+)', $pattern);
                $patternRegex = '#^' . $patternRegex . '$#';
                
                if (preg_match($patternRegex, $uri, $matches)) {
                    // Extract parameters
                    preg_match_all('/{([a-zA-Z0-9_]+)}/', $pattern, $paramNames);
                    $paramNames = $paramNames[1];
                    
                    // Remove first match (full string)
                    array_shift($matches);
                    
                    // Combine param names with values
                    $params = [];
                    foreach ($paramNames as $index => $name) {
                        $params[$name] = $matches[$index];
                    }
                    
                    // Add params to route
                    $route['params'] = $params;
                    
                    return $route;
                }
            }
        }
        
        return false;
    }
}
```

---

## 11. Application Bootstrap

**File: app/core/Application.php**
```php
<?php
namespace App\Core;

use App\Middleware\CSRF;

class Application {
    protected $router;
    protected $config;
    
    public function __construct() {
        $this->router = new Router();
        $this->config = require APP_ROOT . '/config/config.php';
    }
    
    // Register routes
    public function registerRoutes($routes) {
        $this->router->register($routes);
    }
    
    // Process request
    public function processRequest() {
        // Get URI and method
        $uri = $_SERVER['REQUEST_URI'];
        $method = $_SERVER['REQUEST_METHOD'];
        
        // Match route
        $route = $this->router->match($uri, $method);
        
        if (!$route) {
            // Route not found
            $this->handleNotFound();
            return;
        }
        
        // Apply CSRF middleware for all non-GET requests
        if ($method !== 'GET') {
            CSRF::handle();
        }
        
        // Apply route middleware if defined
        if (isset($route['middleware'])) {
            foreach ($route['middleware'] as $middleware) {
                $middlewareClass = "\\App\\Middleware\\{$middleware}";
                if (!$middlewareClass::handle()) {
                    return;
                }
            }
        }
        
        // Get controller and action
        $controllerName = "\\App\\Controllers\\{$route['controller']}";
        $action = $route['action'];
        
        // Check if controller exists
        if (!class_exists($controllerName)) {
            $this->handleNotFound();
            return;
        }
        
        // Create controller instance
        $controller = new $controllerName();
        
        // Check if action exists
        if (!method_exists($controller, $action)) {
            $this->handleNotFound();
            return;
        }
        
        // Call controller action with parameters if any
        if (isset($route['params'])) {
            $content = call_user_func_array([$controller, $action], $route['params']);
        } else {
            $content = $controller->$action();
        }
        
        // Output content
        echo $content;
    }
    
    // Handle 404 errors
    protected function handleNotFound() {
        header("HTTP/1.0 404 Not Found");
        $errorController = new \App\Controllers\ErrorController();
        echo $errorController->notFound();
    }
}
```

---

## 12. Project Configuration

**File: app/config/config.php**
```php
<?php
return [
    // Application settings
    'app_name' => 'LUMIÈRE',
    'app_url' => 'https://lumiere.example.com',
    'environment' => 'development', // 'development', 'production'
    'debug' => true,
    
    // Database configuration
    'database' => require_once __DIR__ . '/database.php',
    
    // Mail configuration
    'mail' => require_once __DIR__ . '/mail.php',
    
    // Payment gateway configuration
    'payment' => require_once __DIR__ . '/payment.php',
    
    // Session configuration
    'session' => [
        'lifetime' => 120, // minutes
        'secure' => false, // true in production
        'http_only' => true,
        'same_site' => 'lax',
    ],
    
    // Cookie configuration
    'cookie' => [
        'expiry' => 30, // days
        'path' => '/',
        'domain' => '',
        'secure' => false, // true in production
        'http_only' => true,
        'same_site' => 'lax',
    ],
    
    // Security configuration
    'security' => [
        'csrf_expiry' => 60, // minutes
        'password_min_length' => 8,
        'throttle_attempts' => 5,
        'throttle_timeout' => 60, // seconds
    ],
    
    // File upload configuration
    'uploads' => [
        'max_size' => 5 * 1024 * 1024, // 5MB
        'allowed_types' => ['image/jpeg', 'image/png', 'image/webp'],
        'product_images_path' => 'uploads/products',
        'category_images_path' => 'uploads/categories',
        'banner_images_path' => 'uploads/banners',
    ],
    
    // Cache configuration
    'cache' => [
        'enabled' => true,
        'driver' => 'file', // 'file', 'redis'
        'lifetime' => 60, // minutes
    ],
    
    // Logging configuration
    'logging' => [
        'level' => 'debug', // 'debug', 'info', 'warning', 'error'
        'file' => __DIR__ . '/../logs/app.log',
    ],
];
```

---

## 13. Implementation Plan

### 13.1 Phase 1: Core Setup (Week 1-2)

1. Set up project structure
2. Implement database schema
3. Create core classes (Application, Router, Database)
4. Implement base controllers and models
5. Set up authentication system

### 13.2 Phase 2: Frontend Foundations (Week 3-4)

1. Implement frontend assets (CSS, JavaScript)
2. Create master layout templates
3. Implement home page
4. Create product listing and detail pages
5. Implement responsive design

### 13.3 Phase 3: E-commerce Functionality (Week 5-6)

1. Implement shopping cart
2. Create checkout process
3. Implement payment gateway integration
4. Add order processing
5. Create user account area

### 13.4 Phase 4: Admin Dashboard (Week 7-8)

1. Create admin layout
2. Implement product management
3. Add order management
4. Create user management
5. Implement dashboard statistics

### 13.5 Phase 5: Testing and Optimization (Week 9-10)

1. Perform unit and integration testing
2. Implement caching strategies
3. Optimize database queries
4. Improve security measures
5. Finalize documentation

---

## 14. Testing Strategy

### 14.1 Unit Testing

Unit tests will be created for all model methods, service methods, and utility functions using PHPUnit. Each test will focus on a single unit of functionality, with dependencies mocked as needed.

### 14.2 Integration Testing

Integration tests will verify that different components work together correctly, such as controllers interacting with models, or services interacting with external APIs.

### 14.3 Front-end Testing

Front-end testing will include:
- Responsive design testing across devices
- Browser compatibility testing
- JavaScript functionality testing
- Accessibility testing using tools like Lighthouse

### 14.4 Security Testing

Security testing will include:
- CSRF protection verification
- SQL injection prevention
- XSS vulnerability scanning
- Input validation testing
- Authentication and authorization testing

### 14.5 Performance Testing

Performance testing will focus on:
- Page load speed optimization
- Database query optimization
- Caching effectiveness
- Server resource usage
- Load testing for concurrent users

---

## 15. Conclusion

This technical design specification document provides a comprehensive blueprint for building the LUMIÈRE aromatherapy e-commerce platform. By following the modular, MVC-inspired architecture outlined in this document, developers can create a secure, maintainable, and extensible application that delivers a premium shopping experience.

The combination of modern PHP 8.3 features, a custom lightweight framework, and responsive frontend design ensures that the platform can grow with the business needs while maintaining performance and security standards.

Junior developers should follow the implementation plan, focusing on one component at a time and ensuring thorough testing at each stage. The detailed file structure, code examples, and design patterns provided should serve as a solid foundation for development.

---

## Appendix A: Database Migration Scripts

```sql
-- Create tables for LUMIÈRE e-commerce platform

-- Users table
CREATE TABLE users (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    role ENUM('customer', 'admin', 'editor') DEFAULT 'customer',
    is_active BOOLEAN DEFAULT TRUE,
    email_verified_at TIMESTAMP NULL,
    remember_token VARCHAR(100),
    reset_token VARCHAR(100),
    reset_token_expiry TIMESTAMP NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    last_login TIMESTAMP NULL
);

-- User addresses table
CREATE TABLE user_addresses (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    user_id INT UNSIGNED NOT NULL,
    address_line1 VARCHAR(255) NOT NULL,
    address_line2 VARCHAR(255),
    city VARCHAR(100) NOT NULL,
    state VARCHAR(100) NOT NULL,
    postal_code VARCHAR(20) NOT NULL,
    country VARCHAR(100) NOT NULL,
    phone VARCHAR(20),
    is_default BOOLEAN DEFAULT FALSE,
    address_type ENUM('billing', 'shipping', 'both') DEFAULT 'both',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Categories table
CREATE TABLE categories (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    parent_id INT UNSIGNED,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    image VARCHAR(255),
    is_active BOOLEAN DEFAULT TRUE,
    sort_order INT UNSIGNED DEFAULT 0,
    meta_title VARCHAR(255),
    meta_description VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE SET NULL
);

-- Brands table
CREATE TABLE brands (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    logo VARCHAR(255),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Tax classes table
CREATE TABLE tax_classes (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    rate DECIMAL(5, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Products table
CREATE TABLE products (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    sku VARCHAR(50) NOT NULL UNIQUE,
    name VARCHAR(255) NOT NULL,
    slug VARCHAR(255) NOT NULL UNIQUE,
    description TEXT,
    short_description VARCHAR(500),
    price DECIMAL(10, 2) NOT NULL,
    sale_price DECIMAL(10, 2),
    cost DECIMAL(10, 2),
    quantity INT UNSIGNED DEFAULT 0,
    is_featured BOOLEAN DEFAULT FALSE,
    is_active BOOLEAN DEFAULT TRUE,
    meta_title VARCHAR(255),
    meta_description VARCHAR(500),
    category_id INT UNSIGNED,
    brand_id INT UNSIGNED,
    tax_class_id INT UNSIGNED,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE SET NULL,
    FOREIGN KEY (brand_id) REFERENCES brands(id) ON DELETE SET NULL,
    FOREIGN KEY (tax_class_id) REFERENCES tax_classes(id) ON DELETE SET NULL
);

-- Product images table
CREATE TABLE product_images (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    product_id INT UNSIGNED NOT NULL,
    image VARCHAR(255) NOT NULL,
    alt_text VARCHAR(255),
    sort_order INT UNSIGNED DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

-- Attributes table
CREATE TABLE attributes (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    display_name VARCHAR(100) NOT NULL,
    type ENUM('select', 'radio', 'checkbox', 'text') NOT NULL,
    sort_order INT UNSIGNED DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Product attributes table
CREATE TABLE product_attributes (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    product_id INT UNSIGNED NOT NULL,
    attribute_id INT UNSIGNED NOT NULL,
    value TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE,
    FOREIGN KEY (attribute_id) REFERENCES attributes(id) ON DELETE CASCADE
);

-- Order statuses table
CREATE TABLE order_statuses (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    color VARCHAR(20) NOT NULL,
    sort_order INT UNSIGNED DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Shipping methods table
CREATE TABLE shipping_methods (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    sort_order INT UNSIGNED DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Payment methods table
CREATE TABLE payment_methods (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    sort_order INT UNSIGNED DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Orders table
CREATE TABLE orders (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    order_number VARCHAR(50) NOT NULL UNIQUE,
    user_id INT UNSIGNED,
    status_id INT UNSIGNED NOT NULL,
    shipping_address_id INT UNSIGNED,
    billing_address_id INT UNSIGNED,
    shipping_method_id INT UNSIGNED,
    payment_method_id INT UNSIGNED,
    subtotal DECIMAL(10, 2) NOT NULL,
    shipping_cost DECIMAL(10, 2) NOT NULL DEFAULT 0,
    tax_amount DECIMAL(10, 2) NOT NULL DEFAULT 0,
    discount_amount DECIMAL(10, 2) NOT NULL DEFAULT 0,
    total DECIMAL(10, 2) NOT NULL,
    notes TEXT,
    tracking_number VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL,
    FOREIGN KEY (status_id) REFERENCES order_statuses(id),
    FOREIGN KEY (shipping_method_id) REFERENCES shipping_methods(id) ON DELETE SET NULL,
    FOREIGN KEY (payment_method_id) REFERENCES payment_methods(id) ON DELETE SET NULL
);

-- Order items table
CREATE TABLE order_items (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    order_id INT UNSIGNED NOT NULL,
    product_id INT UNSIGNED,
    product_name VARCHAR(255) NOT NULL,
    product_sku VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    quantity INT UNSIGNED NOT NULL,
    subtotal DECIMAL(10, 2) NOT NULL,
    tax_amount DECIMAL(10, 2) NOT NULL DEFAULT 0,
    total DECIMAL(10, 2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE SET NULL
);

-- Order status history table
CREATE TABLE order_status_history (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    order_id INT UNSIGNED NOT NULL,
    status_id INT UNSIGNED NOT NULL,
    comment TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE,
    FOREIGN KEY (status_id) REFERENCES order_statuses(id)
);

-- Payments table
CREATE TABLE payments (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    order_id INT UNSIGNED NOT NULL,
    transaction_id VARCHAR(100),
    payment_method VARCHAR(50) NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(50) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (order_id) REFERENCES orders(id) ON DELETE CASCADE
);

-- Reviews table
CREATE TABLE reviews (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    product_id INT UNSIGNED NOT NULL,
    user_id INT UNSIGNED,
    title VARCHAR(255) NOT NULL,
    comment TEXT NOT NULL,
    rating INT UNSIGNED NOT NULL,
    is_approved BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL
);

-- Carts table
CREATE TABLE carts (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    user_id INT UNSIGNED,
    session_id VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Cart items table
CREATE TABLE cart_items (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    cart_id INT UNSIGNED NOT NULL,
    product_id INT UNSIGNED NOT NULL,
    quantity INT UNSIGNED NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (cart_id) REFERENCES carts(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE
);

-- Wishlist table
CREATE TABLE wishlist (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    user_id INT UNSIGNED NOT NULL,
    product_id INT UNSIGNED NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE,
    UNIQUE KEY user_product (user_id, product_id)
);

-- Coupons table
CREATE TABLE coupons (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    code VARCHAR(50) NOT NULL UNIQUE,
    type ENUM('percentage', 'fixed') NOT NULL,
    value DECIMAL(10, 2) NOT NULL,
    min_spend DECIMAL(10, 2),
    starts_at TIMESTAMP NULL,
    expires_at TIMESTAMP NULL,
    is_active BOOLEAN DEFAULT TRUE,
    max_uses INT UNSIGNED,
    used_count INT UNSIGNED DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Subscribers table
CREATE TABLE subscribers (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    is_active BOOLEAN DEFAULT TRUE,
    token VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Settings table
CREATE TABLE settings (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    `key` VARCHAR(100) NOT NULL UNIQUE,
    value TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Page content table
CREATE TABLE page_content (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    slug VARCHAR(100) NOT NULL UNIQUE,
    title VARCHAR(255) NOT NULL,
    content TEXT NOT NULL,
    meta_title VARCHAR(255),
    meta_description VARCHAR(500),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```
