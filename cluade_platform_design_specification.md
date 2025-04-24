# LUMIÈRE E-COMMERCE PLATFORM
## Technical Design Specification Document

**Version:** 1.0  
**Date:** April 23, 2025  
**Author:** Technical Architect Team  

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [System Architecture Overview](#2-system-architecture-overview)
3. [Development Environment Setup](#3-development-environment-setup)
4. [Database Design](#4-database-design)
5. [Application Structure](#5-application-structure)
6. [Core Components](#6-core-components)
7. [User Authentication & Authorization](#7-user-authentication--authorization)
8. [Product Management](#8-product-management)
9. [Shopping Cart & Checkout](#9-shopping-cart--checkout)
10. [Order Processing](#10-order-processing)
11. [Payment Integration](#11-payment-integration)
12. [User Account Management](#12-user-account-management)
13. [Administration Panel](#13-administration-panel)
14. [Frontend Implementation](#14-frontend-implementation)
15. [API Endpoints](#15-api-endpoints)
16. [Security Measures](#16-security-measures)
17. [Performance Optimization](#17-performance-optimization)
18. [Testing Strategy](#18-testing-strategy)
19. [Deployment Process](#19-deployment-process)
20. [Maintenance & Support](#20-maintenance--support)
21. [Appendix](#21-appendix)

---

## 1. Introduction

### 1.1 Project Overview

The LUMIÈRE E-Commerce Platform is a sophisticated online retail system designed specifically for premium aromatherapy and natural beauty products. This platform emphasizes the luxury experience, focusing on elegant design, seamless user experience, and robust backend functionality to support the sale of high-end products.

### 1.2 Project Objectives

- Develop a custom, extensible e-commerce platform without dependency on commercial solutions
- Create a visually stunning frontend that conveys luxury and premium positioning
- Implement secure, efficient backend processes for inventory, orders, and customer management
- Ensure optimal performance across all devices and connection speeds
- Build a platform that can scale as the business grows
- Maintain GDPR compliance and implement robust security measures

### 1.3 Target Audience

- Luxury consumers seeking premium natural beauty and aromatherapy products
- Health and wellness enthusiasts prioritizing high-quality, natural ingredients
- Gift purchasers looking for premium packaged products
- Subscription-based customers who enroll in membership programs

### 1.4 Technology Stack

- **Web Server:** Apache 2.4+
- **Programming Language:** PHP 8.3
- **Architecture:** Custom MVC framework inspired by Laravel principles
- **Database:** MariaDB 11.7
- **Frontend Framework:** Tailwind CSS 3.3+
- **JavaScript:** Vanilla JS with optional Alpine.js for interactivity
- **Build Tools:** Vite for asset compilation
- **Version Control:** Git
- **Deployment:** CI/CD pipeline with GitHub Actions

### 1.5 Scope & Limitations

This specification covers the complete design and implementation of the LUMIÈRE E-Commerce Platform, including customer-facing interfaces and administration tools. Third-party integrations will be limited to payment gateways, shipping providers, and email services to maintain control over the core e-commerce logic.

---

## 2. System Architecture Overview

### 2.1 Architectural Pattern

The LUMIÈRE platform utilizes a custom Model-View-Controller (MVC) architecture inspired by Laravel principles but implemented with lightweight, purpose-built components. This approach provides transparency, control, and performance benefits while maintaining separation of concerns.

### 2.2 Key Architectural Components

#### 2.2.1 Models
Handle data operations and business logic. Each model represents a database entity and encapsulates the methods for interacting with that data.

#### 2.2.2 Views
Generate the HTML output using a templating system that separates presentation from logic. Views will be modular and reusable.

#### 2.2.3 Controllers
Process incoming requests, interact with models, and determine which views to render. Controllers serve as the intermediary between the data (models) and presentation (views).

#### 2.2.4 Routing System
Maps URLs to specific controller actions using clean, semantic routes.

#### 2.2.5 Database Abstraction Layer
Provides a consistent interface for database operations while enabling query optimization.

#### 2.2.6 Authentication System
Manages user sessions, permissions, and access control.

### 2.3 System Interaction Flow

1. The user requests a page through their browser
2. The request is received by Apache and passed to the application's front controller (index.php)
3. The router parses the URL and determines the appropriate controller and action
4. The controller processes the request, interacts with models as needed
5. Models interact with the database using the database abstraction layer
6. The controller prepares data and passes it to the view
7. The view renders HTML, which is returned to the user's browser

### 2.4 System Components Diagram

```
┌─────────────────┐        ┌──────────────┐        ┌───────────────┐
│   Web Browser   │◄──────►│  Web Server  │◄──────►│ Front Controller │
└─────────────────┘        └──────────────┘        └───────┬───────┘
                                                          │
                                                          ▼
┌────────────────┐         ┌──────────────┐        ┌───────────────┐
│     Views      │◄────────┤ Controllers  │◄──────►│    Router     │
└────────────────┘         └──────┬───────┘        └───────────────┘
                                  │
                                  ▼
                           ┌──────────────┐        ┌───────────────┐
                           │    Models    │◄──────►│  Database     │
                           └──────────────┘        └───────────────┘
```

---

## 3. Development Environment Setup

### 3.1 Prerequisites

- PHP 8.3+
- MariaDB 11.7+
- Apache 2.4+ with mod_rewrite enabled
- Composer for PHP dependency management
- Node.js and npm for frontend asset compilation
- Git for version control

### 3.2 Local Development Environment

#### 3.2.1 Installation Steps

1. Clone the repository:
```bash
git clone https://github.com/lumiere/ecommerce.git
cd ecommerce
```

2. Configure the web server to point to the `/public` directory as document root
3. Create a database for the application
4. Copy `/config.example.php` to `/config.php` and set appropriate values
5. Install PHP dependencies:
```bash
composer install
```

6. Install frontend dependencies and compile assets:
```bash
npm install
npm run dev
```

7. Run database migrations and seeders:
```bash
php tools/migrate.php
php tools/seed.php
```

#### 3.2.2 Configuration Settings

Local development settings should be stored in `/config.php`, which is excluded from version control. This file should define constants for:

- Database credentials
- Path constants
- Error reporting settings
- API keys (using environment variables where possible)
- Debug mode flag

### 3.3 Development Workflow

1. Create a feature branch from the main branch
2. Implement changes following coding standards
3. Write tests for new functionality
4. Submit a pull request for code review
5. Merge to main branch after approval
6. Deploy to staging environment for QA

### 3.4 Coding Standards

- PSR-12 for PHP code style
- BEM methodology for CSS class naming
- JSDoc standards for JavaScript documentation
- Descriptive variable and function names
- Maximum line length of 120 characters
- UTF-8 encoding for all files

---

## 4. Database Design

### 4.1 Database Schema

The database design follows normalization principles to ensure data integrity while optimizing for read-heavy operations common in e-commerce platforms.

#### 4.1.1 Core Entities

- Users
- Products
- Categories
- Orders
- Order_Items
- Cart_Items
- Addresses
- Subscriptions
- Payments
- Reviews
- Media
- Inventory
- Attributes
- Product_Attributes

### 4.2 Detailed Table Structures

#### 4.2.1 users

```sql
CREATE TABLE `users` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `email` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `first_name` varchar(100) NOT NULL,
  `last_name` varchar(100) NOT NULL,
  `phone` varchar(20) DEFAULT NULL,
  `role` enum('customer','admin','super_admin') NOT NULL DEFAULT 'customer',
  `status` enum('active','inactive','banned') NOT NULL DEFAULT 'active',
  `verification_token` varchar(255) DEFAULT NULL,
  `is_verified` tinyint(1) NOT NULL DEFAULT 0,
  `last_login` datetime DEFAULT NULL,
  `password_reset_token` varchar(255) DEFAULT NULL,
  `password_reset_expires` datetime DEFAULT NULL,
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `email` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

#### 4.2.2 products

```sql
CREATE TABLE `products` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `sku` varchar(50) NOT NULL,
  `name` varchar(255) NOT NULL,
  `slug` varchar(255) NOT NULL,
  `description` text,
  `short_description` varchar(500) DEFAULT NULL,
  `price` decimal(10,2) NOT NULL,
  `sale_price` decimal(10,2) DEFAULT NULL,
  `cost` decimal(10,2) DEFAULT NULL,
  `quantity` int(11) NOT NULL DEFAULT 0,
  `is_featured` tinyint(1) NOT NULL DEFAULT 0,
  `is_active` tinyint(1) NOT NULL DEFAULT 1,
  `meta_title` varchar(255) DEFAULT NULL,
  `meta_description` varchar(255) DEFAULT NULL,
  `weight` decimal(8,2) DEFAULT NULL,
  `dimensions` varchar(50) DEFAULT NULL,
  `tax_class` varchar(50) DEFAULT NULL,
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `sku` (`sku`),
  UNIQUE KEY `slug` (`slug`),
  KEY `idx_active_featured` (`is_active`, `is_featured`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

#### 4.2.3 categories

```sql
CREATE TABLE `categories` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `parent_id` int(11) unsigned DEFAULT NULL,
  `name` varchar(255) NOT NULL,
  `slug` varchar(255) NOT NULL,
  `description` text,
  `image` varchar(255) DEFAULT NULL,
  `is_active` tinyint(1) NOT NULL DEFAULT 1,
  `sort_order` int(11) NOT NULL DEFAULT 0,
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `slug` (`slug`),
  KEY `parent_id` (`parent_id`),
  CONSTRAINT `fk_category_parent` FOREIGN KEY (`parent_id`) REFERENCES `categories` (`id`) ON DELETE SET NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

#### 4.2.4 product_categories

```sql
CREATE TABLE `product_categories` (
  `product_id` int(11) unsigned NOT NULL,
  `category_id` int(11) unsigned NOT NULL,
  PRIMARY KEY (`product_id`,`category_id`),
  KEY `category_id` (`category_id`),
  CONSTRAINT `fk_pc_product` FOREIGN KEY (`product_id`) REFERENCES `products` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_pc_category` FOREIGN KEY (`category_id`) REFERENCES `categories` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

#### 4.2.5 orders

```sql
CREATE TABLE `orders` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `user_id` int(11) unsigned NOT NULL,
  `order_number` varchar(50) NOT NULL,
  `status` enum('pending','processing','shipped','delivered','cancelled','refunded') NOT NULL DEFAULT 'pending',
  `subtotal` decimal(10,2) NOT NULL,
  `tax` decimal(10,2) NOT NULL DEFAULT 0.00,
  `shipping` decimal(10,2) NOT NULL DEFAULT 0.00,
  `discount` decimal(10,2) NOT NULL DEFAULT 0.00,
  `total` decimal(10,2) NOT NULL,
  `coupon_code` varchar(50) DEFAULT NULL,
  `shipping_address_id` int(11) unsigned DEFAULT NULL,
  `billing_address_id` int(11) unsigned DEFAULT NULL,
  `payment_method` varchar(50) DEFAULT NULL,
  `shipping_method` varchar(50) DEFAULT NULL,
  `notes` text,
  `tracking_number` varchar(100) DEFAULT NULL,
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `order_number` (`order_number`),
  KEY `user_id` (`user_id`),
  KEY `shipping_address_id` (`shipping_address_id`),
  KEY `billing_address_id` (`billing_address_id`),
  CONSTRAINT `fk_order_user` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`),
  CONSTRAINT `fk_order_shipping` FOREIGN KEY (`shipping_address_id`) REFERENCES `addresses` (`id`),
  CONSTRAINT `fk_order_billing` FOREIGN KEY (`billing_address_id`) REFERENCES `addresses` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

#### 4.2.6 order_items

```sql
CREATE TABLE `order_items` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `order_id` int(11) unsigned NOT NULL,
  `product_id` int(11) unsigned NOT NULL,
  `quantity` int(11) NOT NULL,
  `price` decimal(10,2) NOT NULL,
  `subtotal` decimal(10,2) NOT NULL,
  `tax` decimal(10,2) NOT NULL DEFAULT 0.00,
  `total` decimal(10,2) NOT NULL,
  `options` json DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `order_id` (`order_id`),
  KEY `product_id` (`product_id`),
  CONSTRAINT `fk_orderitem_order` FOREIGN KEY (`order_id`) REFERENCES `orders` (`id`) ON DELETE CASCADE,
  CONSTRAINT `fk_orderitem_product` FOREIGN KEY (`product_id`) REFERENCES `products` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### 4.3 Database Relationships Diagram

The entity-relationship diagram details the connections between tables, highlighting primary and foreign keys, and cardinality of relationships.

### 4.4 Indexing Strategy

- Primary keys on all tables
- Foreign keys for all relationships
- Composite indexes for frequently queried combinations
- Full-text indexes for search functionality
- Consider partial indexes for filtered queries

### 4.5 Database Migration Scripts

Migration scripts will be stored in `/database/migrations` and will follow a sequential naming convention: `YYYYMMDD_description.sql`. A custom migration tool (`tools/migrate.php`) will execute these scripts in order.

---

## 5. Application Structure

### 5.1 Directory Structure

```
/
├── config.php                  # Main configuration file
├── config.example.php          # Example configuration template
├── index.php                   # Front controller entry point
├── .htaccess                   # URL rewriting rules
├── composer.json               # PHP dependencies
├── package.json                # Frontend dependencies
├── /public/                    # Publicly accessible files
│   ├── index.php               # Public entry point
│   ├── /css/                   # Compiled CSS files
│   ├── /js/                    # Compiled JavaScript files
│   ├── /images/                # Image assets
│   ├── /videos/                # Video assets
│   ├── /fonts/                 # Font files
│   └── /.htaccess              # Apache configuration
├── /src/                       # Application source code
│   ├── /Controllers/           # Controller classes
│   ├── /Models/                # Model classes
│   ├── /Views/                 # View templates
│   │   └── /layouts/           # Layout templates
│   ├── /Helpers/               # Helper functions
│   ├── /Services/              # Service classes
│   ├── /Exceptions/            # Custom exceptions
│   └── /Middleware/            # Request middleware
├── /includes/                  # Core functionality
│   ├── init.php                # Application initialization
│   ├── db.php                  # Database connection
│   ├── functions.php           # Utility functions
│   ├── Router.php              # Routing class
│   ├── Request.php             # Request handling
│   ├── Response.php            # Response handling
│   ├── Session.php             # Session management
│   ├── Auth.php                # Authentication
│   └── Validator.php           # Input validation
├── /database/                  # Database related files
│   ├── /migrations/            # Database migrations
│   └── /seeds/                 # Database seeders
├── /tests/                     # Test files
├── /logs/                      # Application logs
├── /uploads/                   # User uploaded content
│   ├── /products/              # Product images
│   ├── /users/                 # User profile images
│   └── /temp/                  # Temporary uploads
└── /tools/                     # Command line utilities
    ├── migrate.php             # Database migration tool
    └── seed.php                # Database seeding tool
```

### 5.2 Key File Specifications

#### 5.2.1 /config.php

```php
<?php
// Database configuration
define('DB_HOST', 'localhost');
define('DB_NAME', 'lumiere_db');
define('DB_USER', 'lumiere_user');
define('DB_PASS', 'secure_password');
define('DB_PORT', 3306);
define('DB_CHARSET', 'utf8mb4');

// Path configuration
define('BASE_URL', 'https://lumiere.com');
define('ROOT_PATH', dirname(__FILE__));
define('PUBLIC_PATH', ROOT_PATH . '/public');
define('UPLOADS_PATH', ROOT_PATH . '/uploads');
define('VIEWS_PATH', ROOT_PATH . '/src/Views');

// Application configuration
define('APP_NAME', 'LUMIÈRE');
define('APP_ENV', 'production'); // 'development', 'staging', 'production'
define('DEBUG_MODE', false);
define('SESSION_NAME', 'lumiere_session');
define('SESSION_LIFETIME', 7200); // seconds
define('TIMEZONE', 'UTC');

// Security
define('AUTH_SALT', 'unique_random_string_here');
define('CSRF_TOKEN_NAME', 'lumiere_csrf_token');
define('PASSWORD_ALGO', PASSWORD_ARGON2ID);

// Email configuration
define('MAIL_HOST', 'smtp.example.com');
define('MAIL_PORT', 587);
define('MAIL_USERNAME', 'noreply@lumiere.com');
define('MAIL_PASSWORD', 'email_password');
define('MAIL_ENCRYPTION', 'tls');
define('MAIL_FROM_ADDRESS', 'noreply@lumiere.com');
define('MAIL_FROM_NAME', 'LUMIÈRE');

// Payment gateway configuration
define('STRIPE_PUBLIC_KEY', 'pk_live_xxxxx');
define('STRIPE_SECRET_KEY', 'sk_live_xxxxx');
define('PAYPAL_CLIENT_ID', 'client_id_xxxxx');
define('PAYPAL_CLIENT_SECRET', 'client_secret_xxxxx');
define('PAYMENT_MODE', 'live'); // 'sandbox', 'live'

// Shopping cart
define('CART_COOKIE_NAME', 'lumiere_cart');
define('CART_COOKIE_LIFETIME', 604800); // 7 days in seconds

// Pagination
define('ITEMS_PER_PAGE', 12);

// API keys
define('GOOGLE_MAPS_API_KEY', 'xxxxx');
define('RECAPTCHA_SITE_KEY', 'xxxxx');
define('RECAPTCHA_SECRET_KEY', 'xxxxx');

// Feature flags
define('ENABLE_REVIEWS', true);
define('ENABLE_WISHLISTS', true);
define('ENABLE_SUBSCRIPTIONS', true);
```

#### 5.2.2 /includes/db.php

```php
<?php
/**
 * Database connection handler
 * Provides a PDO connection to the database and utility methods
 */
class Database {
    private static $instance = null;
    private $connection;
    private $transactionLevel = 0;
    
    /**
     * Private constructor to enforce singleton pattern
     */
    private function __construct() {
        $dsn = 'mysql:host=' . DB_HOST . ';dbname=' . DB_NAME . ';charset=' . DB_CHARSET;
        $options = [
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
            PDO::ATTR_EMULATE_PREPARES => false,
            PDO::MYSQL_ATTR_FOUND_ROWS => true
        ];
        
        try {
            $this->connection = new PDO($dsn, DB_USER, DB_PASS, $options);
        } catch (PDOException $e) {
            // Log the error but don't expose details in production
            error_log('Database connection error: ' . $e->getMessage());
            
            if (DEBUG_MODE) {
                throw new Exception('Database connection failed: ' . $e->getMessage());
            } else {
                throw new Exception('System error: Database connection failed');
            }
        }
    }
    
    /**
     * Get database instance (singleton pattern)
     *
     * @return Database
     */
    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }
    
    /**
     * Get the PDO connection object
     *
     * @return PDO
     */
    public function getConnection() {
        return $this->connection;
    }
    
    /**
     * Execute a query and return the statement
     *
     * @param string $query SQL query with placeholders
     * @param array $params Parameters to bind to query
     * @return PDOStatement
     */
    public function query($query, $params = []) {
        try {
            $stmt = $this->connection->prepare($query);
            $stmt->execute($params);
            return $stmt;
        } catch (PDOException $e) {
            $this->handleQueryError($e, $query, $params);
        }
    }
    
    /**
     * Fetch a single row from the database
     *
     * @param string $query SQL query with placeholders
     * @param array $params Parameters to bind to query
     * @param int $fetchMode PDO fetch mode
     * @return array|null Row data or null if no results
     */
    public function fetchOne($query, $params = [], $fetchMode = PDO::FETCH_ASSOC) {
        try {
            $stmt = $this->connection->prepare($query);
            $stmt->execute($params);
            $result = $stmt->fetch($fetchMode);
            return $result === false ? null : $result;
        } catch (PDOException $e) {
            $this->handleQueryError($e, $query, $params);
        }
    }
    
    /**
     * Fetch multiple rows from the database
     *
     * @param string $query SQL query with placeholders
     * @param array $params Parameters to bind to query
     * @param int $fetchMode PDO fetch mode
     * @return array Array of rows
     */
    public function fetchAll($query, $params = [], $fetchMode = PDO::FETCH_ASSOC) {
        try {
            $stmt = $this->connection->prepare($query);
            $stmt->execute($params);
            return $stmt->fetchAll($fetchMode);
        } catch (PDOException $e) {
            $this->handleQueryError($e, $query, $params);
        }
    }
    
    /**
     * Insert a record into the database
     *
     * @param string $table Table name
     * @param array $data Associative array of column => value
     * @return int|bool Last insert ID or false on failure
     */
    public function insert($table, $data) {
        $columns = implode(', ', array_keys($data));
        $placeholders = implode(', ', array_fill(0, count($data), '?'));
        
        $query = "INSERT INTO {$table} ({$columns}) VALUES ({$placeholders})";
        
        try {
            $stmt = $this->connection->prepare($query);
            $stmt->execute(array_values($data));
            return $this->connection->lastInsertId();
        } catch (PDOException $e) {
            $this->handleQueryError($e, $query, array_values($data));
            return false;
        }
    }
    
    /**
     * Update records in the database
     *
     * @param string $table Table name
     * @param array $data Associative array of column => value
     * @param string $where WHERE clause (without the "WHERE" keyword)
     * @param array $params Parameters for WHERE clause
     * @return int Number of affected rows
     */
    public function update($table, $data, $where, $params = []) {
        $set = [];
        foreach ($data as $column => $value) {
            $set[] = "{$column} = ?";
        }
        
        $setClause = implode(', ', $set);
        $query = "UPDATE {$table} SET {$setClause} WHERE {$where}";
        
        try {
            $stmt = $this->connection->prepare($query);
            $stmt->execute(array_merge(array_values($data), $params));
            return $stmt->rowCount();
        } catch (PDOException $e) {
            $this->handleQueryError($e, $query, array_merge(array_values($data), $params));
            return 0;
        }
    }
    
    /**
     * Delete records from the database
     *
     * @param string $table Table name
     * @param string $where WHERE clause (without the "WHERE" keyword)
     * @param array $params Parameters for WHERE clause
     * @return int Number of affected rows
     */
    public function delete($table, $where, $params = []) {
        $query = "DELETE FROM {$table} WHERE {$where}";
        
        try {
            $stmt = $this->connection->prepare($query);
            $stmt->execute($params);
            return $stmt->rowCount();
        } catch (PDOException $e) {
            $this->handleQueryError($e, $query, $params);
            return 0;
        }
    }
    
    /**
     * Begin a transaction
     *
     * @return bool True on success
     */
    public function beginTransaction() {
        if ($this->transactionLevel == 0) {
            $this->connection->beginTransaction();
        }
        $this->transactionLevel++;
        return true;
    }
    
    /**
     * Commit a transaction
     *
     * @return bool True on success
     */
    public function commit() {
        $this->transactionLevel--;
        if ($this->transactionLevel == 0) {
            return $this->connection->commit();
        }
        return true;
    }
    
    /**
     * Rollback a transaction
     *
     * @return bool True on success
     */
    public function rollback() {
        if ($this->transactionLevel > 0) {
            $this->transactionLevel = 0;
            return $this->connection->rollBack();
        }
        return false;
    }
    
    /**
     * Count rows from a table based on conditions
     *
     * @param string $table Table name
     * @param string $where WHERE clause (without the "WHERE" keyword)
     * @param array $params Parameters for WHERE clause
     * @return int Row count
     */
    public function count($table, $where = '', $params = []) {
        $query = "SELECT COUNT(*) as count FROM {$table}";
        if (!empty($where)) {
            $query .= " WHERE {$where}";
        }
        
        try {
            $stmt = $this->connection->prepare($query);
            $stmt->execute($params);
            $result = $stmt->fetch(PDO::FETCH_ASSOC);
            return (int) $result['count'];
        } catch (PDOException $e) {
            $this->handleQueryError($e, $query, $params);
            return 0;
        }
    }
    
    /**
     * Handle database query errors
     *
     * @param PDOException $e Exception object
     * @param string $query SQL query that caused the error
     * @param array $params Parameters used in the query
     * @throws Exception Rethrows a sanitized exception
     */
    private function handleQueryError(PDOException $e, $query, $params) {
        // Log the detailed error
        $logMessage = "Database query error: " . $e->getMessage() . "\n";
        $logMessage .= "Query: " . $query . "\n";
        $logMessage .= "Params: " . print_r($params, true);
        error_log($logMessage);
        
        // In debug mode, provide detailed error info
        if (DEBUG_MODE) {
            throw new Exception('Database query error: ' . $e->getMessage() . ' in query: ' . $query);
        } else {
            // In production, provide a generic error
            throw new Exception('System error: Database operation failed');
        }
    }
}
```

#### 5.2.3 /public/index.php

```php
<?php
/**
 * Front Controller
 * All requests are routed through this file
 */

// Set error reporting based on environment
error_reporting(E_ALL);
ini_set('display_errors', 0);

// Define application start time for performance monitoring
define('APP_START', microtime(true));

// Load configuration and initialize the application
require_once dirname(__DIR__) . '/config.php';
require_once dirname(__DIR__) . '/includes/init.php';

// Set timezone
date_default_timezone_set(TIMEZONE);

// Start the session
session_name(SESSION_NAME);
session_start();

// Create request and response objects
$request = new Request();
$response = new Response();

// Initialize the router
$router = new Router($request, $response);

// Load routes
require_once dirname(__DIR__) . '/src/routes.php';

// Dispatch the request to the appropriate controller
try {
    $router->dispatch();
} catch (Exception $e) {
    // Log the error
    error_log($e->getMessage());
    
    // Handle the exception based on request type and environment
    if ($request->isAjax()) {
        // For AJAX requests, return JSON error
        http_response_code(500);
        echo json_encode(['error' => DEBUG_MODE ? $e->getMessage() : 'An error occurred']);
    } else {
        // For regular requests, show error page
        if (DEBUG_MODE) {
            // Show detailed error in development
            echo '<h1>Error</h1>';
            echo '<p>' . $e->getMessage() . '</p>';
            echo '<h2>Stack Trace</h2>';
            echo '<pre>' . $e->getTraceAsString() . '</pre>';
        } else {
            // Show friendly error page in production
            http_response_code(500);
            require VIEWS_PATH . '/errors/500.php';
        }
    }
}

// Log performance metrics if in debug mode
if (DEBUG_MODE) {
    $executionTime = microtime(true) - APP_START;
    error_log("Request to {$_SERVER['REQUEST_URI']} completed in {$executionTime} seconds");
}
```

#### 5.2.4 /src/routes.php

```php
<?php
/**
 * Application Routes
 * Define all URL routes and their corresponding controllers/actions
 */

// Home page
$router->get('/', 'HomeController@index');

// Authentication routes
$router->get('/login', 'AuthController@loginForm');
$router->post('/login', 'AuthController@login');
$router->get('/register', 'AuthController@registerForm');
$router->post('/register', 'AuthController@register');
$router->get('/forgot-password', 'AuthController@forgotPasswordForm');
$router->post('/forgot-password', 'AuthController@forgotPassword');
$router->get('/reset-password/{token}', 'AuthController@resetPasswordForm');
$router->post('/reset-password', 'AuthController@resetPassword');
$router->get('/logout', 'AuthController@logout');
$router->get('/verify-email/{token}', 'AuthController@verifyEmail');

// Product routes
$router->get('/products', 'ProductController@index');
$router->get('/products/{slug}', 'ProductController@show');
$router->get('/category/{slug}', 'CategoryController@show');
$router->get('/search', 'SearchController@index');

// Cart routes
$router->get('/cart', 'CartController@index');
$router->post('/cart/add', 'CartController@add');
$router->post('/cart/update', 'CartController@update');
$router->post('/cart/remove', 'CartController@remove');
$router->get('/cart/clear', 'CartController@clear');

// Checkout routes
$router->get('/checkout', 'CheckoutController@index');
$router->post('/checkout/process', 'CheckoutController@process');
$router->get('/checkout/success/{order_number}', 'CheckoutController@success');
$router->get('/checkout/cancel', 'CheckoutController@cancel');

// User account routes
$router->get('/account', 'AccountController@index');
$router->get('/account/orders', 'AccountController@orders');
$router->get('/account/orders/{order_number}', 'AccountController@orderDetail');
$router->get('/account/addresses', 'AccountController@addresses');
$router->post('/account/addresses/add', 'AccountController@addAddress');
$router->post('/account/addresses/update', 'AccountController@updateAddress');
$router->post('/account/addresses/delete', 'AccountController@deleteAddress');
$router->get('/account/profile', 'AccountController@profile');
$router->post('/account/profile/update', 'AccountController@updateProfile');
$router->get('/account/password', 'AccountController@passwordForm');
$router->post('/account/password/update', 'AccountController@updatePassword');
$router->get('/account/wishlist', 'WishlistController@index');
$router->post('/account/wishlist/add', 'WishlistController@add');
$router->post('/account/wishlist/remove', 'WishlistController@remove');
$router->get('/account/subscriptions', 'SubscriptionController@index');
$router->get('/account/subscriptions/{id}', 'SubscriptionController@show');
$router->post('/account/subscriptions/cancel', 'SubscriptionController@cancel');

// Reviews
$router->post('/reviews/add', 'ReviewController@add');

// Static pages
$router->get('/about', 'PageController@about');
$router->get('/contact', 'PageController@contactForm');
$router->post('/contact', 'PageController@contact');
$router->get('/faq', 'PageController@faq');
$router->get('/terms', 'PageController@terms');
$router->get('/privacy', 'PageController@privacy');
$router->get('/shipping', 'PageController@shipping');
$router->get('/returns', 'PageController@returns');

// Membership routes
$router->get('/membership', 'MembershipController@index');
$router->get('/membership/{plan}', 'MembershipController@plan');
$router->post('/membership/subscribe', 'MembershipController@subscribe');

// API routes
$router->get('/api/products', 'Api\\ProductController@index');
$router->get('/api/products/{id}', 'Api\\ProductController@show');
$router->get('/api/categories', 'Api\\CategoryController@index');
$router->post('/api/contact', 'Api\\ContactController@send');
$router->post('/api/newsletter/subscribe', 'Api\\NewsletterController@subscribe');

// Admin routes
$router->get('/admin', 'Admin\\DashboardController@index');
$router->get('/admin/login', 'Admin\\AuthController@loginForm');
$router->post('/admin/login', 'Admin\\AuthController@login');
$router->get('/admin/logout', 'Admin\\AuthController@logout');

// Admin - Products
$router->get('/admin/products', 'Admin\\ProductController@index');
$router->get('/admin/products/create', 'Admin\\ProductController@create');
$router->post('/admin/products/store', 'Admin\\ProductController@store');
$router->get('/admin/products/edit/{id}', 'Admin\\ProductController@edit');
$router->post('/admin/products/update/{id}', 'Admin\\ProductController@update');
$router->post('/admin/products/delete/{id}', 'Admin\\ProductController@delete');

// Admin - Categories
$router->get('/admin/categories', 'Admin\\CategoryController@index');
$router->get('/admin/categories/create', 'Admin\\CategoryController@create');
$router->post('/admin/categories/store', 'Admin\\CategoryController@store');
$router->get('/admin/categories/edit/{id}', 'Admin\\CategoryController@edit');
$router->post('/admin/categories/update/{id}', 'Admin\\CategoryController@update');
$router->post('/admin/categories/delete/{id}', 'Admin\\CategoryController@delete');

// Admin - Orders
$router->get('/admin/orders', 'Admin\\OrderController@index');
$router->get('/admin/orders/{id}', 'Admin\\OrderController@show');
$router->post('/admin/orders/status/{id}', 'Admin\\OrderController@updateStatus');

// Admin - Users
$router->get('/admin/users', 'Admin\\UserController@index');
$router->get('/admin/users/create', 'Admin\\UserController@create');
$router->post('/admin/users/store', 'Admin\\UserController@store');
$router->get('/admin/users/edit/{id}', 'Admin\\UserController@edit');
$router->post('/admin/users/update/{id}', 'Admin\\UserController@update');
$router->post('/admin/users/delete/{id}', 'Admin\\UserController@delete');

// Admin - Settings
$router->get('/admin/settings', 'Admin\\SettingController@index');
$router->post('/admin/settings/update', 'Admin\\SettingController@update');

// Admin - Reports
$router->get('/admin/reports/sales', 'Admin\\ReportController@sales');
$router->get('/admin/reports/products', 'Admin\\ReportController@products');
$router->get('/admin/reports/customers', 'Admin\\ReportController@customers');

// 404 Fallback - This should be the last route
$router->fallback(function() {
    http_response_code(404);
    require VIEWS_PATH . '/errors/404.php';
});
```

---

## 6. Core Components

This section details the major components of the application's core framework.

### 6.1 Router Class

The Router handles URL parsing and dispatches requests to the appropriate controllers. It maps URLs to controller actions and supports RESTful routing patterns.

**File: /includes/Router.php**

```php
<?php
/**
 * Router Class
 * Handles URL routing and dispatching to controllers
 */
class Router {
    private $request;
    private $response;
    private $routes = [
        'GET' => [],
        'POST' => [],
        'PUT' => [],
        'DELETE' => []
    ];
    private $fallbackHandler = null;
    
    /**
     * Constructor
     *
     * @param Request $request Request object
     * @param Response $response Response object
     */
    public function __construct(Request $request, Response $response) {
        $this->request = $request;
        $this->response = $response;
    }
    
    /**
     * Add a GET route
     *
     * @param string $path URL path
     * @param mixed $handler Controller action or callback function
     * @return Router
     */
    public function get($path, $handler) {
        $this->addRoute('GET', $path, $handler);
        return $this;
    }
    
    /**
     * Add a POST route
     *
     * @param string $path URL path
     * @param mixed $handler Controller action or callback function
     * @return Router
     */
    public function post($path, $handler) {
        $this->addRoute('POST', $path, $handler);
        return $this;
    }
    
    /**
     * Add a PUT route
     *
     * @param string $path URL path
     * @param mixed $handler Controller action or callback function
     * @return Router
     */
    public function put($path, $handler) {
        $this->addRoute('PUT', $path, $handler);
        return $this;
    }
    
    /**
     * Add a DELETE route
     *
     * @param string $path URL path
     * @param mixed $handler Controller action or callback function
     * @return Router
     */
    public function delete($path, $handler) {
        $this->addRoute('DELETE', $path, $handler);
        return $this;
    }
    
    /**
     * Set a fallback handler for 404 errors
     *
     * @param callable $handler Function to handle 404 errors
     * @return Router
     */
    public function fallback($handler) {
        $this->fallbackHandler = $handler;
        return $this;
    }
    
    /**
     * Add a route to the routes array
     *
     * @param string $method HTTP method
     * @param string $path URL path
     * @param mixed $handler Controller action or callback function
     */
    private function addRoute($method, $path, $handler) {
        // Convert path parameters to regex pattern
        $pattern = $this->pathToRegex($path);
        
        $this->routes[$method][$pattern] = [
            'handler' => $handler,
            'path' => $path
        ];
    }
    
    /**
     * Convert a path with parameters to regex pattern
     *
     * @param string $path URL path with parameters
     * @return string Regex pattern
     */
    private function pathToRegex($path) {
        $pattern = preg_replace('/\/{([^\/]+)}/', '/(?P<$1>[^/]+)', $path);
        return '#^' . $pattern . '$#';
    }
    
    /**
     * Match the current request to a route
     *
     * @return array|false Route info or false if no match
     */
    private function matchRoute() {
        $method = $this->request->getMethod();
        $uri = $this->request->getUri();
        
        // Remove query string from URI
        if (($pos = strpos($uri, '?')) !== false) {
            $uri = substr($uri, 0, $pos);
        }
        
        // Remove trailing slash except for root path
        if ($uri !== '/' && substr($uri, -1) === '/') {
            $uri = rtrim($uri, '/');
        }
        
        // Check if the route exists for this HTTP method
        if (!isset($this->routes[$method])) {
            return false;
        }
        
        // Match URI against route patterns
        foreach ($this->routes[$method] as $pattern => $route) {
            if (preg_match($pattern, $uri, $matches)) {
                // Extract parameter values
                $params = array_filter($matches, function($key) {
                    return !is_numeric($key);
                }, ARRAY_FILTER_USE_KEY);
                
                return [
                    'handler' => $route['handler'],
                    'params' => $params
                ];
            }
        }
        
        return false;
    }
    
    /**
     * Dispatch the request to the appropriate controller
     */
    public function dispatch() {
        $route = $this->matchRoute();
        
        if ($route) {
            $handler = $route['handler'];
            $params = $route['params'];
            
            // If handler is a string like 'ControllerName@methodName'
            if (is_string($handler) && strpos($handler, '@') !== false) {
                list($controller, $method) = explode('@', $handler);
                
                // Add namespace if not provided
                if (strpos($controller, '\\') === false) {
                    $controller = 'App\\Controllers\\' . $controller;
                }
                
                // Instantiate controller
                $controllerInstance = new $controller($this->request, $this->response);
                
                // Call the controller method with parameters
                call_user_func_array([$controllerInstance, $method], $params);
            } 
            // If handler is a callback function
            elseif (is_callable($handler)) {
                call_user_func_array($handler, $params);
            } 
            else {
                throw new Exception('Invalid route handler');
            }
        } 
        // No matching route found, use fallback handler
        elseif (is_callable($this->fallbackHandler)) {
            call_user_func($this->fallbackHandler);
        } 
        // No fallback handler defined
        else {
            http_response_code(404);
            echo '404 - Page Not Found';
        }
    }
}
```

### 6.2 Request Class

The Request class encapsulates HTTP request data and provides methods to access request parameters, headers, and other information.

**File: /includes/Request.php**

```php
<?php
/**
 * Request Class
 * Handles HTTP request data
 */
class Request {
    private $parameters = [];
    private $cookies = [];
    private $files = [];
    private $server = [];
    
    /**
     * Constructor
     */
    public function __construct() {
        $this->parameters = array_merge($_GET, $_POST);
        $this->cookies = $_COOKIE;
        $this->files = $_FILES;
        $this->server = $_SERVER;
        
        // Handle JSON requests
        $contentType = $this->getContentType();
        if (strpos($contentType, 'application/json') !== false) {
            $json = file_get_contents('php://input');
            if (!empty($json)) {
                $data = json_decode($json, true);
                if (json_last_error() === JSON_ERROR_NONE) {
                    $this->parameters = array_merge($this->parameters, $data);
                }
            }
        }
    }
    
    /**
     * Get the HTTP method
     *
     * @return string
     */
    public function getMethod() {
        return $this->server['REQUEST_METHOD'] ?? 'GET';
    }
    
    /**
     * Get the request URI
     *
     * @return string
     */
    public function getUri() {
        $uri = $this->server['REQUEST_URI'] ?? '/';
        $baseUrl = dirname($this->server['SCRIPT_NAME']);
        
        // Remove base URL from URI if it exists
        if ($baseUrl !== '/' && strpos($uri, $baseUrl) === 0) {
            $uri = substr($uri, strlen($baseUrl));
        }
        
        return '/' . ltrim($uri, '/');
    }
    
    /**
     * Get a request parameter
     *
     * @param string $key Parameter name
     * @param mixed $default Default value if parameter doesn't exist
     * @return mixed
     */
    public function get($key, $default = null) {
        return $this->parameters[$key] ?? $default;
    }
    
    /**
     * Get all request parameters
     *
     * @return array
     */
    public function all() {
        return $this->parameters;
    }
    
    /**
     * Check if a parameter exists
     *
     * @param string $key Parameter name
     * @return bool
     */
    public function has($key) {
        return isset($this->parameters[$key]);
    }
    
    /**
     * Get a cookie value
     *
     * @param string $key Cookie name
     * @param mixed $default Default value if cookie doesn't exist
     * @return mixed
     */
    public function cookie($key, $default = null) {
        return $this->cookies[$key] ?? $default;
    }
    
    /**
     * Get an uploaded file
     *
     * @param string $key File input name
     * @return array|null
     */
    public function file($key) {
        return $this->files[$key] ?? null;
    }
    
    /**
     * Get a server parameter
     *
     * @param string $key Server parameter name
     * @param mixed $default Default value if parameter doesn't exist
     * @return mixed
     */
    public function server($key, $default = null) {
        return $this->server[$key] ?? $default;
    }
    
    /**
     * Get request content type
     *
     * @return string
     */
    public function getContentType() {
        return $this->server('CONTENT_TYPE', '');
    }
    
    /**
     * Check if the request is an AJAX request
     *
     * @return bool
     */
    public function isAjax() {
        return $this->server('HTTP_X_REQUESTED_WITH') === 'XMLHttpRequest';
    }
    
    /**
     * Check if the request is over HTTPS
     *
     * @return bool
     */
    public function isSecure() {
        return $this->server('HTTPS') === 'on' || $this->server('HTTP_X_FORWARDED_PROTO') === 'https';
    }
    
    /**
     * Get the client IP address
     *
     * @return string
     */
    public function getIp() {
        $keys = [
            'HTTP_CLIENT_IP',
            'HTTP_X_FORWARDED_FOR',
            'HTTP_X_FORWARDED',
            'HTTP_X_CLUSTER_CLIENT_IP',
            'HTTP_FORWARDED_FOR',
            'HTTP_FORWARDED',
            'REMOTE_ADDR'
        ];
        
        foreach ($keys as $key) {
            if ($this->server($key)) {
                return $this->server($key);
            }
        }
        
        return '0.0.0.0';
    }
    
    /**
     * Get the user agent string
     *
     * @return string
     */
    public function getUserAgent() {
        return $this->server('HTTP_USER_AGENT', '');
    }
    
    /**
     * Get request header
     *
     * @param string $key Header name
     * @param mixed $default Default value if header doesn't exist
     * @return mixed
     */
    public function header($key, $default = null) {
        $headerKey = 'HTTP_' . strtoupper(str_replace('-', '_', $key));
        return $this->server($headerKey, $default);
    }
}
```

### 6.3 Response Class

The Response class handles HTTP responses, including headers, content, redirects, and JSON responses.

**File: /includes/Response.php**

```php
<?php
/**
 * Response Class
 * Handles HTTP responses
 */
class Response {
    private $headers = [];
    private $content = '';
    private $statusCode = 200;
    
    /**
     * Set response content
     *
     * @param string $content Response content
     * @return Response
     */
    public function setContent($content) {
        $this->content = $content;
        return $this;
    }
    
    /**
     * Set response status code
     *
     * @param int $statusCode HTTP status code
     * @return Response
     */
    public function setStatusCode($statusCode) {
        $this->statusCode = $statusCode;
        return $this;
    }
    
    /**
     * Add a response header
     *
     * @param string $name Header name
     * @param string $value Header value
     * @return Response
     */
    public function addHeader($name, $value) {
        $this->headers[$name] = $value;
        return $this;
    }
    
    /**
     * Send the response
     */
    public function send() {
        // Set status code
        http_response_code($this->statusCode);
        
        // Set headers
        foreach ($this->headers as $name => $value) {
            header("$name: $value");
        }
        
        // Output content
        echo $this->content;
        exit;
    }
    
    /**
     * Send a JSON response
     *
     * @param mixed $data Data to encode as JSON
     * @param int $statusCode HTTP status code
     * @return Response
     */
    public function json($data, $statusCode = 200) {
        $this->setStatusCode($statusCode);
        $this->addHeader('Content-Type', 'application/json');
        $this->setContent(json_encode($data));
        return $this->send();
    }
    
    /**
     * Redirect to another URL
     *
     * @param string $url URL to redirect to
     * @param int $statusCode HTTP status code
     * @return Response
     */
    public function redirect($url, $statusCode = 302) {
        $this->setStatusCode($statusCode);
        $this->addHeader('Location', $url);
        return $this->send();
    }
    
    /**
     * Send a 404 Not Found response
     *
     * @param string $content Response content
     * @return Response
     */
    public function notFound($content = '404 Not Found') {
        $this->setStatusCode(404);
        $this->setContent($content);
        return $this->send();
    }
    
    /**
     * Send a 500 Internal Server Error response
     *
     * @param string $content Response content
     * @return Response
     */
    public function serverError($content = '500 Internal Server Error') {
        $this->setStatusCode(500);
        $this->setContent($content);
        return $this->send();
    }
    
    /**
     * Send a 401 Unauthorized response
     *
     * @param string $content Response content
     * @return Response
     */
    public function unauthorized($content = '401 Unauthorized') {
        $this->setStatusCode(401);
        $this->setContent($content);
        return $this->send();
    }
    
    /**
     * Send a 403 Forbidden response
     *
     * @param string $content Response content
     * @return Response
     */
    public function forbidden($content = '403 Forbidden') {
        $this->setStatusCode(403);
        $this->setContent($content);
        return $this->send();
    }
    
    /**
     * Return a file download
     *
     * @param string $filePath Path to the file
     * @param string $fileName Name for the downloaded file
     * @param string $contentType Content type header
     * @return Response
     */
    public function download($filePath, $fileName = null, $contentType = null) {
        if (!file_exists($filePath)) {
            return $this->notFound();
        }
        
        $fileName = $fileName ?: basename($filePath);
        $contentType = $contentType ?: mime_content_type($filePath);
        
        $this->addHeader('Content-Type', $contentType);
        $this->addHeader('Content-Disposition', 'attachment; filename="' . $fileName . '"');
        $this->addHeader('Content-Length', filesize($filePath));
        $this->addHeader('Cache-Control', 'no-cache, must-revalidate');
        $this->addHeader('Expires', '0');
        $this->addHeader('Pragma', 'public');
        
        readfile($filePath);
        exit;
    }
    
    /**
     * Output a view template
     *
     * @param string $view View name
     * @param array $data Data to pass to the view
     * @param int $statusCode HTTP status code
     * @return Response
     */
    public function view($view, $data = [], $statusCode = 200) {
        $this->setStatusCode($statusCode);
        
        // Extract data to make variables available in view
        extract($data);
        
        // Start output buffering
        ob_start();
        
        // Include the view file
        $viewPath = VIEWS_PATH . '/' . $view . '.php';
        if (file_exists($viewPath)) {
            include $viewPath;
        } else {
            throw new Exception("View not found: $view");
        }
        
        // Get buffer contents and clean the buffer
        $content = ob_get_clean();
        
        $this->setContent($content);
        return $this->send();
    }
}
```

### 6.4 Authentication Class

The Authentication class handles user authentication, including login, logout, and access control.

**File: /includes/Auth.php**

```php
<?php
/**
 * Authentication Class
 * Handles user authentication and authorization
 */
class Auth {
    private static $instance = null;
    private $user = null;
    private $db;
    
    /**
     * Private constructor to enforce singleton pattern
     */
    private function __construct() {
        $this->db = Database::getInstance();
        $this->checkSession();
    }
    
    /**
     * Get authentication instance (singleton pattern)
     *
     * @return Auth
     */
    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }
    
    /**
     * Check if a user is logged in via session
     */
    private function checkSession() {
        if (isset($_SESSION['user_id'])) {
            $userId = $_SESSION['user_id'];
            $query = "SELECT * FROM users WHERE id = ? AND status = 'active' LIMIT 1";
            $user = $this->db->fetchOne($query, [$userId]);
            
            if ($user) {
                $this->user = $user;
            } else {
                // Invalid session, clear it
                $this->logout();
            }
        }
    }
    
    /**
     * Attempt to log in a user
     *
     * @param string $email User email
     * @param string $password User password
     * @param bool $remember Use remember me functionality
     * @return bool Success or failure
     */
    public function login($email, $password, $remember = false) {
        $query = "SELECT * FROM users WHERE email = ? AND status = 'active' LIMIT 1";
        $user = $this->db->fetchOne($query, [$email]);
        
        if ($user && password_verify($password, $user['password'])) {
            // Set session
            $_SESSION['user_id'] = $user['id'];
            
            // Set remember me cookie if requested
            if ($remember) {
                $token = bin2hex(random_bytes(32));
                $expires = time() + (86400 * 30); // 30 days
                
                // Store token in database
                $this->db->update('users', 
                    ['remember_token' => $token], 
                    'id = ?', 
                    [$user['id']]
                );
                
                // Set cookie
                setcookie('remember_token', $token, $expires, '/', '', true, true);
            }
            
            // Update last login time
            $this->db->update('users', 
                ['last_login' => date('Y-m-d H:i:s')], 
                'id = ?', 
                [$user['id']]
            );
            
            $this->user = $user;
            return true;
        }
        
        return false;
    }
    
    /**
     * Log out the current user
     */
    public function logout() {
        // Clear session
        unset($_SESSION['user_id']);
        
        // Clear remember me cookie
        if (isset($_COOKIE['remember_token'])) {
            setcookie('remember_token', '', time() - 3600, '/', '', true, true);
        }
        
        // Regenerate session ID
        session_regenerate_id(true);
        
        $this->user = null;
    }
    
    /**
     * Register a new user
     *
     * @param array $userData User data
     * @return int|bool User ID on success, false on failure
     */
    public function register($userData) {
        // Check if email already exists
        $query = "SELECT id FROM users WHERE email = ? LIMIT 1";
        $existingUser = $this->db->fetchOne($query, [$userData['email']]);
        
        if ($existingUser) {
            return false;
        }
        
        // Hash password
        $userData['password'] = password_hash($userData['password'], PASSWORD_ALGO);
        
        // Generate verification token if verification is required
        if (!isset($userData['is_verified']) || !$userData['is_verified']) {
            $userData['verification_token'] = bin2hex(random_bytes(32));
            $userData['is_verified'] = 0;
        }
        
        // Insert user
        $userId = $this->db->insert('users', $userData);
        
        return $userId;
    }
    
    /**
     * Verify a user's email
     *
     * @param string $token Verification token
     * @return bool Success or failure
     */
    public function verifyEmail($token) {
        $query = "SELECT id FROM users WHERE verification_token = ? LIMIT 1";
        $user = $this->db->fetchOne($query, [$token]);
        
        if ($user) {
            $updated = $this->db->update('users', 
                [
                    'is_verified' => 1, 
                    'verification_token' => null
                ], 
                'id = ?', 
                [$user['id']]
            );
            
            return $updated > 0;
        }
        
        return false;
    }
    
    /**
     * Generate a password reset token
     *
     * @param string $email User email
     * @return string|bool Token or false on failure
     */
    public function generatePasswordResetToken($email) {
        $query = "SELECT id FROM users WHERE email = ? AND status = 'active' LIMIT 1";
        $user = $this->db->fetchOne($query, [$email]);
        
        if ($user) {
            $token = bin2hex(random_bytes(32));
            $expires = date('Y-m-d H:i:s', time() + 3600); // 1 hour
            
            $updated = $this->db->update('users', 
                [
                    'password_reset_token' => $token,
                    'password_reset_expires' => $expires
                ], 
                'id = ?', 
                [$user['id']]
            );
            
            return $updated > 0 ? $token : false;
        }
        
        return false;
    }
    
    /**
     * Reset a user's password
     *
     * @param string $token Reset token
     * @param string $password New password
     * @return bool Success or failure
     */
    public function resetPassword($token, $password) {
        $query = "SELECT id FROM users WHERE password_reset_token = ? AND password_reset_expires > NOW() LIMIT 1";
        $user = $this->db->fetchOne($query, [$token]);
        
        if ($user) {
            $updated = $this->db->update('users', 
                [
                    'password' => password_hash($password, PASSWORD_ALGO),
                    'password_reset_token' => null,
                    'password_reset_expires' => null
                ], 
                'id = ?', 
                [$user['id']]
            );
            
            return $updated > 0;
        }
        
        return false;
    }
    
    /**
     * Check if a user is logged in
     *
     * @return bool
     */
    public function isLoggedIn() {
        return $this->user !== null;
    }
    
    /**
     * Get the current user
     *
     * @return array|null User data or null if not logged in
     */
    public function getUser() {
        return $this->user;
    }
    
    /**
     * Get current user ID
     *
     * @return int|null User ID or null if not logged in
     */
    public function getUserId() {
        return $this->user ? $this->user['id'] : null;
    }
    
    /**
     * Check if the current user has a specific role
     *
     * @param string|array $roles Role(s) to check
     * @return bool
     */
    public function hasRole($roles) {
        if (!$this->isLoggedIn()) {
            return false;
        }
        
        if (is_string($roles)) {
            $roles = [$roles];
        }
        
        return in_array($this->user['role'], $roles);
    }
    
    /**
     * Create a new admin user
     *
     * @param array $userData Admin user data
     * @return int|bool User ID on success, false on failure
     */
    public function createAdmin($userData) {
        $userData['role'] = 'admin';
        $userData['is_verified'] = 1;
        
        return $this->register($userData);
    }
}
```

These core component files establish the foundation of the custom MVC framework, handling routing, request processing, response generation, and user authentication.

---

## 7. User Authentication & Authorization

### 7.1 Authentication Flow

1. **Registration Process**
   - User submits registration form
   - System validates input
   - If email verification is enabled, a verification email is sent
   - User account is created in 'unverified' status
   - User is redirected to login page with success message

2. **Login Process**
   - User submits login credentials
   - System verifies email and password
   - If 'remember me' is checked, a persistent cookie is set
   - User session is created
   - User is redirected to account dashboard or previous page

3. **Password Reset**
   - User requests password reset via email
   - System generates a time-limited token and sends reset link
   - User sets new password
   - All existing sessions are invalidated
   - User is redirected to login with success message

### 7.2 Session Management

- Session data stored in encrypted cookies
- Sessions expire after configured timeout (default: 2 hours)
- Sessions are automatically extended during activity
- One-time CSRF tokens for all state-changing operations
- Session variables include user ID and last activity timestamp

### 7.3 Access Control

1. **Role-Based Access**
   - Each user has a role: customer, admin, or super_admin
   - Routes can be protected by role
   - Admin panel access restricted to admin roles only

2. **Permission Checking**
   - Controller methods check user authorization before execution
   - Users can only access their own data
   - Middleware intercepts unauthorized requests

3. **Example Authorization Code in Controllers**

```php
// In OrderController.php
public function show($orderId) {
    // Get current user
    $userId = Auth::getInstance()->getUserId();
    
    // Load order
    $order = $this->orderModel->findById($orderId);
    
    // Check if order belongs to user or user is admin
    if (!$order || ($order['user_id'] != $userId && !Auth::getInstance()->hasRole(['admin', 'super_admin']))) {
        return $this->response->forbidden('You are not authorized to view this order');
    }
    
    // Continue with order display
    return $this->response->view('orders/show', ['order' => $order]);
}
```

---

## 8. Product Management

### 8.1 Product Structure

Products in the LUMIÈRE platform are structured to support the unique needs of aromatherapy and natural beauty products.

1. **Basic Product Properties**
   - SKU (unique identifier)
   - Name
   - Slug (URL-friendly name)
   - Short description
   - Full description (HTML supported)
   - Price and sale price
   - Quantity in stock
   - Status (active/inactive)
   - Featured flag
   - Categories

2. **Aromatherapy-Specific Properties**
   - Scent profile (e.g., floral, woody, citrus)
   - Key ingredients
   - Benefit tags (e.g., relaxation, energy, sleep)
   - Usage instructions
   - Recommended complementary products

3. **Media Management**
   - Multiple high-resolution product images
   - Product videos (demonstration, usage)
   - 360° view integration
   - Ingredient source imagery

### 8.2 Category System

The category system is hierarchical, allowing for nested categories to organize the product catalog.

1. **Category Structure**
   - Parent category (optional)
   - Name
   - Slug
   - Description
   - Image
   - Sort order

2. **Category Relationships**
   - Products can belong to multiple categories
   - Categories can have multiple child categories
   - Categories have breadcrumb navigation

### 8.3 Product Variations

The platform supports product variations like size, concentration, and packaging options.

1. **Attribute System**
   - Attribute types (e.g., size, scent, packaging)
   - Attribute values (e.g., 30ml, 100ml)

2. **Variation Management**
   - Each variation has its own SKU, price, and inventory
   - Variations share basic product information
   - Option selection interface on product pages

### 8.4 Inventory Management

Inventory is tracked at the product and variation level with these features:

1. **Stock Management**
   - Real-time inventory updates
   - Low stock alerts
   - Backorder capabilities

2. **Batch/Lot Tracking**
   - Track product batches for natural products
   - Expiration date tracking
   - First-in, first-out (FIFO) inventory management

---

## 9. Shopping Cart & Checkout

### 9.1 Cart Implementation

The shopping cart system persists cart data for both logged-in and guest users.

1. **Cart Storage**
   - For logged-in users: database storage
   - For guest users: cookie-based storage
   - Merging cart on login if both exist

2. **Cart Operations**
   - Add to cart (with quantity and options)
   - Update quantity
   - Remove item
   - Clear cart
   - Save for later
   - Move to wishlist

3. **Cart Calculations**
   - Subtotal per item
   - Cart subtotal
   - Shipping cost estimation
   - Tax calculation
   - Discounts and coupon codes
   - Order total

### 9.2 Checkout Flow

The checkout process is streamlined while capturing necessary information.

1. **Checkout Steps**
   - Cart review
   - Customer information (login or guest)
   - Shipping address
   - Shipping method selection
   - Payment method selection
   - Order review
   - Order confirmation

2. **Guest Checkout**
   - Email required for order status updates
   - Optional account creation after purchase
   - All other customer details required

3. **Order Placement**
   - Final inventory check
   - Payment processing
   - Order confirmation email
   - Redirect to thank you page with order details

### 9.3 Address Management

Customers can manage multiple shipping and billing addresses.

1. **Address Structure**
   - First and last name
   - Company name (optional)
   - Street address (line 1 and 2)
   - City
   - State/Province
   - Postal code
   - Country
   - Phone number
   - Default shipping/billing flags

2. **Address Operations**
   - Add new address
   - Edit existing address
   - Delete address
   - Set default shipping/billing address

---

## 10. Order Processing

### 10.1 Order Lifecycle

Orders flow through several states from creation to fulfillment.

1. **Order Statuses**
   - Pending (initial state)
   - Processing (payment confirmed)
   - Shipped (order dispatched)
   - Delivered (order received)
   - Cancelled (order cancelled)
   - Refunded (payment returned)

2. **Status Transitions**
   - Status changes logged with timestamp and user
   - Email notifications for status changes
   - Customer can view order status in account

### 10.2 Order Management

The admin panel provides tools for order processing.

1. **Order Actions**
   - View order details
   - Update order status
   - Add order notes (internal and customer-facing)
   - Generate invoice
   - Generate packing slip
   - Process refunds
   - Cancel order

2. **Fulfillment Process**
   - Mark items as picked
   - Record tracking information
   - Send shipping confirmation
   - Batch process orders

### 10.3 Returns & Refunds

The platform supports customer returns and refunds.

1. **Return Process**
   - Customer initiates return request
   - Admin approves or denies request
   - Return shipping label generation
   - Return tracking
   - Refund processing upon receipt

2. **Refund Types**
   - Full refund
   - Partial refund
   - Store credit
   - Replacement item

---

## 11. Payment Integration

### 11.1 Payment Gateway Integration

The platform integrates with popular payment gateways to process transactions securely.

1. **Supported Gateways**
   - Stripe (primary)
   - PayPal
   - Apple Pay / Google Pay
   - Bank transfers

2. **Payment Processing**
   - Real-time payment validation
   - Fraud detection
   - Payment tokenization
   - Subscription billing

### 11.2 Secure Payment Handling

Security measures for payment processing adhere to PCI-DSS requirements.

1. **Security Features**
   - No credit card data stored on server
   - Payment tokenization
   - 3D Secure support
   - Address Verification System (AVS)
   - HTTPS throughout checkout
   - CSRF protection

2. **Transaction Logging**
   - Transaction ID
   - Amount
   - Status
   - Gateway response
   - Error messages

### 11.3 Subscription Billing

The platform supports recurring billing for membership and subscription products.

1. **Subscription Management**
   - Billing cycles (monthly, quarterly, yearly)
   - Free trial periods
   - Subscription pause/resume
   - Automatic renewals
   - Upgrade/downgrade options

2. **Billing Operations**
   - Initial subscription setup
   - Recurring billing
   - Failed payment handling
   - Expiration/renewal notifications
   - Cancellation processing

---

## 12. User Account Management

### 12.1 User Profiles

Customer accounts store personal information and preferences.

1. **Profile Information**
   - Personal details (name, email, phone)
   - Communication preferences
   - Product preferences
   - Account settings

2. **Profile Management**
   - Update personal information
   - Change password
   - Email preferences
   - Delete account request

### 12.2 Order History

Customers can view their complete order history.

1. **Order Listing**
   - Order number
   - Date
   - Status
   - Total amount
   - Shipping information

2. **Order Details**
   - Line items with prices
   - Shipping and billing addresses
   - Payment information
   - Tracking information
   - Order notes
   - Reorder functionality

### 12.3 Wishlist & Saved Items

Customers can save products for future purchase.

1. **Wishlist Features**
   - Add/remove products
   - Share wishlist
   - Move to cart
   - Price change notifications

2. **Saved Carts**
   - Save current cart for later
   - Multiple named carts
   - Scheduled reorder

---

## 13. Administration Panel

### 13.1 Dashboard

The admin dashboard provides a quick overview of site performance.

1. **Dashboard Widgets**
   - Recent orders
   - Sales statistics
   - Low stock alerts
   - Customer registrations
   - Sales by category/product
   - Revenue graph

2. **Quick Actions**
   - Process orders
   - Add products
   - View reports
   - Manage promotions

### 13.2 Product Management

Administrators can manage the product catalog.

1. **Product CRUD Operations**
   - Create new products
   - Edit existing products
   - Clone products
   - Delete/deactivate products
   - Bulk operations

2. **Product Import/Export**
   - CSV/Excel import
   - Data validation
   - Error handling
   - Scheduled imports

### 13.3 User Management

Administrators can manage customer accounts.

1. **User Operations**
   - View customer details
   - Edit customer information
   - View customer orders
   - Disable/enable accounts
   - Reset passwords
   - Manage admin accounts

2. **Customer Groups**
   - Define customer groups
   - Set group permissions
   - Group-based pricing
   - Group-based content

### 13.4 Content Management

Administrators can manage site content.

1. **Page Management**
   - Edit static pages
   - Create new pages
   - Delete pages
   - SEO settings

2. **Media Library**
   - Upload images/videos
   - Organize media
   - Optimize images
   - Set alt text

### 13.5 Settings & Configuration

Administrators can configure the platform settings.

1. **System Settings**
   - Store information
   - Currency settings
   - Tax configuration
   - Email templates
   - Shipping methods
   - Payment gateways

2. **User Interface Settings**
   - Theme customization
   - Navigation menus
   - Homepage layout
   - Mobile appearance

---

## 14. Frontend Implementation

### 14.1 Responsive Design

The frontend is designed to be fully responsive across all device sizes.

1. **Responsive Approach**
   - Mobile-first design using Tailwind CSS
   - Responsive grid system
   - Flexible images and media
   - Touch-friendly interface
   - Device-specific optimizations

2. **Breakpoints**
   - Small: 0-576px (mobile phones)
   - Medium: 577-768px (tablets)
   - Large: 769-992px (laptops)
   - Extra Large: 993-1200px (desktops)
   - XXL: 1201px+ (large screens)

### 14.2 Day/Night Mode Toggle

The platform implements a theme toggle for light and dark modes.

1. **Theme Implementation**
   - CSS variables for color schemes
   - JavaScript for theme switching
   - Local storage for preference saving
   - System preference detection
   - Smooth transitions between modes

2. **Theme-Specific Assets**
   - Adjusted images for dark mode
   - Icon variations
   - Contrast considerations
   - Readability focus

### 14.3 Animations & Microinteractions

Subtle animations enhance the user experience without being distracting.

1. **Animation Types**
   - Hover effects
   - Transition animations
   - Loading states
   - Scroll-triggered animations
   - Attention-guiding motions

2. **Performance Considerations**
   - GPU-accelerated properties
   - Reduced motion preference support
   - Lazy-loaded animations
   - Animation throttling

### 14.4 Frontend Optimization

Performance optimizations ensure a fast, smooth user experience.

1. **Performance Techniques**
   - Lazy loading images and videos
   - Code splitting
   - Critical CSS inlining
   - Font optimizations
   - Asset minification
   - Resource hints (preconnect, prefetch)

2. **Caching Strategy**
   - Browser caching
   - Service worker for offline support
   - Asset versioning
   - Cache invalidation

---

## 15. API Endpoints

### 15.1 Public API

The platform provides public API endpoints for integration with third-party systems.

1. **Product Endpoints**
   - GET /api/products: List products
   - GET /api/products/{id}: Get product details
   - GET /api/categories: List categories
   - GET /api/categories/{id}: Get category details
   - GET /api/search: Search products

2. **Authentication**
   - API key authentication
   - Rate limiting
   - IP whitelisting

### 15.2 Private API

Authenticated API endpoints for customer accounts and ordering.

1. **User Endpoints**
   - POST /api/auth/login: User login
   - POST /api/auth/register: User registration
   - GET /api/account: Get account details
   - GET /api/orders: Get order history
   - GET /api/orders/{id}: Get order details

2. **Cart & Checkout**
   - GET /api/cart: Get cart contents
   - POST /api/cart/add: Add to cart
   - PUT /api/cart/update: Update cart item
   - DELETE /api/cart/remove: Remove cart item
   - POST /api/checkout: Process checkout

3. **Authentication & Security**
   - JWT token authentication
   - CSRF protection
   - Permission checking

---

## 16. Security Measures

### 16.1 Data Protection

The platform implements comprehensive data protection measures.

1. **Personal Data Handling**
   - GDPR compliance
   - Data minimization
   - Purpose limitation
   - Data encryption
   - Privacy policy

2. **Database Security**
   - Parameterized queries
   - Database user privileges
   - Data encryption at rest
   - Regular backups

### 16.2 XSS & CSRF Prevention

Protection against common web vulnerabilities.

1. **XSS Prevention**
   - Output escaping
   - Content Security Policy (CSP)
   - Input validation
   - HTML purification

2. **CSRF Protection**
   - Per-session CSRF tokens
   - Same-site cookies
   - Origin validation
   - Automatic token validation

### 16.3 Authentication Security

Secure authentication mechanisms protect user accounts.

1. **Password Security**
   - Argon2id hashing
   - Password strength requirements
   - Account lockout after failed attempts
   - Secure password reset

2. **Session Security**
   - Secure, HTTP-only cookies
   - Session timeouts
   - Session invalidation on logout
   - Session fixation protection

### 16.4 Security Headers

HTTP headers that enhance security.

1. **Security Headers**
   - Content-Security-Policy
   - X-XSS-Protection
   - X-Content-Type-Options
   - Strict-Transport-Security
   - Referrer-Policy

2. **Implementation**
   - Server configuration
   - Application-level headers
   - Regular security scans

---

## 17. Performance Optimization

### 17.1 Database Optimization

Techniques to ensure database efficiency.

1. **Query Optimization**
   - Indexed columns for frequent queries
   - Query caching
   - Optimized JOIN operations
   - Pagination
   - Database profiling

2. **Database Structure**
   - Normalization for data integrity
   - Strategic denormalization for performance
   - Appropriate data types
   - Partitioning for large tables

### 17.2 Caching

Multiple layers of caching improve response times.

1. **Caching Layers**
   - Database query cache
   - Object cache
   - Page cache
   - Browser cache
   - CDN caching

2. **Cache Invalidation**
   - Time-based expiration
   - Event-based invalidation
   - Cache tags
   - Manual purging

### 17.3 Asset Optimization

Optimization techniques for static assets.

1. **Image Optimization**
   - WebP format with fallbacks
   - Responsive images (srcset)
   - Lazy loading
   - Image compression
   - Optimal dimensions

2. **CSS/JS Optimization**
   - Minification
   - Concatenation
   - Tree shaking
   - Critical CSS
   - Deferred loading

---

## 18. Testing Strategy

### 18.1 Unit Testing

Testing individual components in isolation.

1. **Testing Scope**
   - Models
   - Services
   - Helper functions
   - Utility classes

2. **Testing Tools**
   - PHPUnit for PHP components
   - Jest for JavaScript components

### 18.2 Integration Testing

Testing component interactions.

1. **Testing Scope**
   - Controller actions
   - Database operations
   - Third-party integrations
   - API endpoints

2. **Testing Approach**
   - Mock external services
   - Database fixtures
   - Test authentication flows

### 18.3 User Acceptance Testing

Validating the system meets business requirements.

1. **Testing Scope**
   - End-to-end flows
   - Business processes
   - User scenarios

2. **Testing Approach**
   - Scripted scenarios
   - Real user testing
   - Feature validation

### 18.4 Performance Testing

Ensuring the system performs well under load.

1. **Testing Types**
   - Load testing
   - Stress testing
   - Endurance testing
   - Spike testing

2. **Testing Tools**
   - JMeter
   - Artillery
   - Lighthouse
   - WebPageTest

---

## 19. Deployment Process

### 19.1 Environment Setup

Configuration of different environments.

1. **Environment Types**
   - Development: Local environment for developers
   - Staging: Pre-production environment for testing
   - Production: Live environment for customers

2. **Environment Configuration**
   - Environment-specific settings
   - Feature flags
   - Database credentials
   - Service endpoints

### 19.2 Deployment Pipeline

Automated deployment process.

1. **CI/CD Pipeline**
   - Code repository (GitHub)
   - Continuous Integration (GitHub Actions)
   - Automated testing
   - Build process
   - Deployment

2. **Deployment Steps**
   - Code checkout
   - Dependency installation
   - Asset compilation
   - Database migrations
   - Configuration updates
   - Service restart
   - Cache warming

### 19.3 Rollback Procedures

Process for handling failed deployments.

1. **Rollback Triggers**
   - Failed deployment
   - Post-deployment errors
   - Performance degradation
   - Security vulnerabilities

2. **Rollback Process**
   - Revert to previous version
   - Database rollback
   - Configuration rollback
   - Service restart
   - Monitoring

---

## 20. Maintenance & Support

### 20.1 Monitoring

Continuous monitoring of system health.

1. **Monitoring Aspects**
   - Server health (CPU, memory, disk)
   - Application errors
   - Database performance
   - Page load times
   - Transaction success rates

2. **Monitoring Tools**
   - New Relic
   - Datadog
   - Custom logging
   - Error tracking

### 20.2 Backup Strategy

Data backup procedures to prevent loss.

1. **Backup Types**
   - Database backups
   - File system backups
   - Configuration backups

2. **Backup Schedule**
   - Daily incremental backups
   - Weekly full backups
   - Monthly archives
   - Pre-deployment backups

### 20.3 Update Procedures

Process for applying system updates.

1. **Update Types**
   - Security patches
   - Bug fixes
   - Feature enhancements
   - Third-party dependencies

2. **Update Process**
   - Update staging environment
   - Test thoroughly
   - Schedule production update
   - Apply update
   - Verify functionality
   - Monitor for issues

---

## 21. Appendix

### 21.1 Technology Stack Details

Detailed information about technology versions and compatibility.

1. **Server Requirements**
   - Apache 2.4+ with mod_rewrite
   - PHP 8.3+
   - MariaDB 11.7+
   - 2GB+ RAM
   - 20GB+ disk space

2. **PHP Extensions**
   - PDO and PDO_MySQL
   - mbstring
   - curl
   - gd
   - xml
   - zip
   - intl
   - opcache

### 21.2 Coding Standards

Coding standards ensure consistency across the codebase.

1. **PHP Coding Standards**
   - PSR-12 coding style
   - PSR-4 autoloading
   - Docblock comments
   - Type hints
   - Exception handling

2. **JavaScript Coding Standards**
   - ES6+ syntax
   - Proper DOM manipulation
   - Event delegation
   - Error handling
   - Performance considerations

### 21.3 Project Timeline

Estimated timeline for project implementation.

1. **Phase 1: Core Development (4 weeks)**
   - System architecture
   - Database design
   - User authentication
   - Admin foundation

2. **Phase 2: Product Catalog (3 weeks)**
   - Product management
   - Category system
   - Search functionality
   - Media management

3. **Phase 3: Shopping & Checkout (3 weeks)**
   - Cart implementation
   - Checkout process
   - Payment integration
   - Order management

4. **Phase 4: User Experience (3 weeks)**
   - Frontend implementation
   - Responsive design
   - User account features
   - Performance optimization

5. **Phase 5: Testing & Deployment (2 weeks)**
   - Comprehensive testing
   - Bug fixes
   - Security audits
   - Production deployment

### 21.4 Resource Requirements

Human and technical resources needed for project implementation.

1. **Development Team**
   - 1 Technical Lead
   - 2 Backend Developers
   - 1 Frontend Developer
   - 1 UI/UX Designer
   - 1 QA Engineer

2. **Infrastructure Resources**
   - Development environment
   - Staging environment
   - Production environment
   - CI/CD pipeline
   - Monitoring systems

---

This comprehensive technical design specification document outlines the architecture, design, and implementation details of the LUMIÈRE E-Commerce Platform. By following these specifications, developers can build a modular, secure, and extensible platform that delivers a premium shopping experience for aromatherapy and natural beauty products.
