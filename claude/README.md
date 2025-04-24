# LUMIÈRE 

<p align="center">
  <img src="https://raw.githubusercontent.com/yourusername/lumiere/main/public/images/lumiere-logo.png" alt="LUMIÈRE Logo" width="400">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/PHP-8.3-blueviolet" alt="PHP 8.3">
  <img src="https://img.shields.io/badge/Laravel-12.0-red" alt="Laravel 12">
  <img src="https://img.shields.io/badge/MariaDB-11.7-blue" alt="MariaDB 11.7">
  <img src="https://img.shields.io/badge/Tailwind-3.3-38b2ac" alt="Tailwind CSS 3.3">
  <img src="https://img.shields.io/badge/License-MIT-green" alt="License">
</p>

<p align="center">
  <i>A premium, artisanal e-commerce platform for luxury aromatherapy products.</i>
</p>

---

## ✨ Project Overview

LUMIÈRE is a sophisticated e-commerce platform designed specifically for premium aromatherapy and natural beauty products. Built with a focus on sensory excellence and user experience, LUMIÈRE redefines online shopping for luxury wellness products through thoughtful design, seamless functionality, and performance optimization.

The platform embraces a custom MVC-inspired architecture with carefully chosen modern technologies, creating a transparent, maintainable, and scalable codebase that prioritizes developer control while delivering exceptional frontend experiences.

---

## 🖼️ Visual Experience

<p align="center">
  <img src="https://raw.githubusercontent.com/yourusername/lumiere/main/docs/screenshots/homepage.png" alt="LUMIÈRE Homepage" width="800">
</p>

<details>
  <summary><strong>View more screenshots</strong></summary>
  <p align="center">
    <img src="https://raw.githubusercontent.com/yourusername/lumiere/main/docs/screenshots/product-listing.png" alt="Product Listing" width="400">
    <img src="https://raw.githubusercontent.com/yourusername/lumiere/main/docs/screenshots/product-detail.png" alt="Product Detail" width="400">
  </p>
  <p align="center">
    <img src="https://raw.githubusercontent.com/yourusername/lumiere/main/docs/screenshots/checkout.png" alt="Checkout Process" width="400">
    <img src="https://raw.githubusercontent.com/yourusername/lumiere/main/docs/screenshots/admin-dashboard.png" alt="Admin Dashboard" width="400">
  </p>
</details>

[View Live Demo](https://lumiere-demo.example.com) | [Video Walkthrough](https://youtu.be/example)

---

## 🌟 The LUMIÈRE Experience

### A Visitor's Journey

From the moment visitors land on LUMIÈRE, they are immersed in an atmosphere of refined luxury and sensorial delight. The experience begins with a captivating full-screen video background showing botanical ingredients in slow motion, paired with subtle particle animations that respond to cursor movements. The hero section introduces the brand ethos with an elegant fade-in animation, establishing an immediate emotional connection.

As visitors scroll, they encounter a thoughtfully curated grid of featured collections, each image transforming subtly on hover to invite exploration. The bestselling products section showcases each item with sophisticated 3D hover effects and micro-interactions that enhance product discovery while maintaining visual harmony.

The website's color palette shifts gracefully between day and night modes, offering a personalized viewing experience while maintaining brand integrity through carefully selected accent colors and typography pairings that exude timeless elegance.

Navigation throughout the site is intuitive yet elevated by subtle animations and transitions that guide the user's journey without distraction. The shopping experience features:

- Custom cursor effects that enhance interactive elements
- Context-aware product recommendations
- Seamless filtering and sorting capabilities
- Detailed product pages with immersive imagery and informative content
- A streamlined checkout process designed to minimize friction and abandonment

Behind the beautiful interface, the platform delivers exceptional performance with instant page transitions, optimized image loading, and responsive design that provides a consistent experience across all devices.

### The Admin Experience

Store administrators enjoy a powerful yet intuitive dashboard that provides comprehensive control over:

- Inventory management with detailed product information
- Order processing and fulfillment tracking
- Customer relationship management
- Content customization for marketing materials
- Detailed analytics and reporting
- Marketing campaign management

The admin interface maintains the same design language as the customer-facing site while prioritizing efficiency and clarity for operational tasks.

---

## 💭 Design Philosophy

### Core Principles

The LUMIÈRE platform is built upon four foundational design principles:

1. **Sensory Excellence**: Every interaction is designed to delight the senses, mirroring the aromatherapeutic experience of the products themselves.

2. **Timeless Sophistication**: The design balances contemporary digital trends with classic elegance to create an enduring aesthetic that won't quickly feel dated.

3. **Intentional Minimalism**: Elements are carefully curated to avoid overwhelming the user, allowing products and brand storytelling to take center stage.

4. **Thoughtful Motion**: Animations and transitions serve a purpose—enhancing understanding, guiding attention, or providing feedback—never merely decorative.

### Visual Language

The visual language of LUMIÈRE is defined by:

#### Typography System
- **Playfair Display**: Primary headline font conveying heritage and luxury
- **Cinzel**: Used for accent headings and logo treatments
- **Jost**: Modern, geometric sans-serif for body copy and UI elements
- **Cormorant Garamond**: Elegant serif for quotes and editorial content

#### Color Philosophy
The color palette is intentionally restrained, allowing the rich visuals of the products to shine:

- **Day Mode**: Airy neutrals (ivory, soft cream) with champagne gold accents (#d4af37)
- **Night Mode**: Deep indigo canvas with emerald green accents (#50d890)

This duality reflects the dual nature of aromatherapy—energizing by day, calming by night.

#### Material Treatment
Surface treatments include:
- Subtle texture overlays mimicking fine paper
- Soft shadows creating depth without heaviness
- Glass-morphism effects suggesting the transparency of essential oils
- Gradient transitions inspired by the diffusion of scent

### Interaction Design

Micro-interactions throughout the site reinforce brand identity:
- Custom cursor effects that change state on interactive elements
- Particle animations suggesting aromatic molecules
- Subtle hover states that reveal additional information
- Loading animations inspired by the diffusion of essential oils

---

## 🛠️ Technical Architecture

### Technology Stack Overview

LUMIÈRE employs a carefully selected technology stack that balances modern development practices with performance and maintainability:

**Server Environment**:
- Apache 2.4+ web server
- PHP 8.3+ for server-side processing
- MariaDB 11.7+ for data storage
- Redis (optional) for enhanced caching

**Frontend Technologies**:
- HTML5 with semantic markup
- Tailwind CSS 3.3+ for utility-first styling
- Vanilla JavaScript with strategic use of Alpine.js
- GSAP for advanced animations
- Vanilla Lazyload for image optimization

**Backend Framework**:
- Custom MVC architecture inspired by Laravel 12
- Composer for dependency management
- PHPMailer for email functionality
- Intervention Image for image processing
- Monolog for logging

**Development Tools**:
- Git for version control
- PHPUnit for testing
- NPM for asset management
- Laravel Mix for asset compilation

### Architecture Design

The platform employs a custom implementation of the Model-View-Controller (MVC) pattern that maintains separation of concerns while providing flexibility:

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Browser   │───▶│    Router   │───▶│ Controllers │
└─────────────┘    └─────────────┘    └─────────────┘
                                             │
┌─────────────┐    ┌─────────────┐           │
│    Views    │◀───│  Services   │◀──────────┘
└─────────────┘    └─────────────┘           │
                          ▲                   │
                          │                   ▼
                          │            ┌─────────────┐
                          └────────────│   Models    │
                                       └─────────────┘
                                             │
                                             ▼
                                      ┌─────────────┐
                                      │  Database   │
                                      └─────────────┘
```

**Request Lifecycle**:
1. The browser initiates a request to a specific route
2. The Router matches the request to the appropriate Controller and action
3. Middleware is applied for authentication, CSRF protection, etc.
4. The Controller interacts with Models and Services to process business logic
5. Views render the response using data provided by the Controller
6. The response is returned to the browser

This architecture enables:
- Clear separation of concerns
- Testable components
- Easy maintenance and extension
- Efficient code reuse

### Database Design

The database schema is designed for flexibility, performance, and data integrity:

- Normalized design with appropriate indexes
- Foreign key constraints for referential integrity
- Database transactions for data consistency
- Optimized query patterns

Key entities include:
- Users and authentication
- Products and categories
- Orders and payments
- Content management
- Customer relationships

---

## 🎨 Frontend Implementation

### Responsive Design Strategy

LUMIÈRE implements a mobile-first responsive design approach using a combination of:

- Fluid typography with clamp() for optimal reading experiences
- CSS Grid and Flexbox for layout
- Strategic breakpoints at 640px, 768px, 1024px, and 1280px
- Container queries for component-level responsiveness
- `prefers-reduced-motion` media query support for accessibility

### Performance Optimization

Frontend performance is optimized through:

```javascript
// Intersection Observer for efficient lazy loading
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('visible');
            observer.unobserve(entry.target);
        }
    });
}, {
    threshold: 0.15,
    rootMargin: '0px 0px -100px 0px'
});

fadeElements.forEach(element => {
    observer.observe(element);
});
```

- Modern image formats (WebP/AVIF) with appropriate fallbacks
- Critical CSS inlining
- Deferred loading of non-critical assets
- Dynamic import() for JavaScript modules
- Resource hints for preloading and preconnecting
- Efficient bundling and minification

### Accessibility Considerations

The platform prioritizes accessibility through:

- Semantic HTML structure
- ARIA attributes for enhanced screen reader support
- Keyboard navigation support
- Focus management
- Sufficient color contrast (WCAG AA compliance)
- Alternative text for images
- Skip navigation links
- Reduced motion options

### Component Design

UI components are developed with a focus on:

- Reusability across the platform
- Consistent API and behavior
- Self-contained styling
- Progressive enhancement
- Clear documentation

---

## ⚙️ Backend Implementation

### Custom MVC Framework

The backend architecture is built on a lightweight custom MVC framework inspired by Laravel but optimized for transparency and control:

**Models** encapsulate database operations and business logic:

```php
class Product extends BaseModel {
    protected $table = 'products';
    protected $fillable = [
        'sku', 'name', 'slug', 'description', 'short_description', 
        'price', 'sale_price', 'cost', 'quantity', 'is_featured',
        'is_active', 'meta_title', 'meta_description', 'category_id', 
        'brand_id', 'tax_class_id'
    ];
    
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
    
    // Additional methods...
}
```

**Controllers** process user input and prepare responses:

```php
class ProductController extends BaseController {
    protected $productModel;
    protected $categoryModel;
    
    public function __construct() {
        $this->productModel = new Product();
        $this->categoryModel = new Category();
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
        
        // Get related products
        $relatedProducts = $this->productModel->getRelatedProducts(
            $product['id'],
            $product['category_id'],
            4
        );
        
        $data = [
            'title' => $product['name'] . ' | LUMIÈRE',
            'product' => $product,
            'images' => $images,
            'relatedProducts' => $relatedProducts,
            'csrf_token' => $this->generateToken()
        ];
        
        return $this->view('products/detail', $data);
    }
    
    // Additional methods...
}
```

**Views** render HTML using data from controllers:

```php
<!-- Featured products section -->
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
                    <!-- Product display logic -->
                </div>
            <?php endforeach; ?>
        </div>
    </div>
</section>
```

### Service Layer

The service layer encapsulates complex business logic that spans multiple models:

```php
class PaymentService {
    protected $orderModel;
    protected $paymentModel;
    
    public function __construct() {
        $this->orderModel = new Order();
        $this->paymentModel = new Payment();
    }
    
    public function processPayment($orderId, $paymentData) {
        // Begin database transaction
        $this->orderModel->db->beginTransaction();
        
        try {
            // Process payment with gateway
            $gatewayResponse = $this->processWithGateway($paymentData);
            
            if (!$gatewayResponse['success']) {
                throw new Exception($gatewayResponse['message']);
            }
            
            // Record payment in database
            $paymentRecord = [
                'order_id' => $orderId,
                'transaction_id' => $gatewayResponse['transaction_id'],
                'payment_method' => $paymentData['method'],
                'amount' => $paymentData['amount'],
                'status' => 'completed'
            ];
            
            $paymentId = $this->paymentModel->create($paymentRecord);
            
            if (!$paymentId) {
                throw new Exception('Failed to record payment');
            }
            
            // Update order status
            $orderUpdateSuccess = $this->orderModel->updateStatus(
                $orderId, 
                OrderStatus::PAYMENT_RECEIVED
            );
            
            if (!$orderUpdateSuccess) {
                throw new Exception('Failed to update order status');
            }
            
            // Commit transaction
            $this->orderModel->db->commit();
            
            return [
                'success' => true,
                'payment_id' => $paymentId,
                'transaction_id' => $gatewayResponse['transaction_id']
            ];
            
        } catch (Exception $e) {
            // Rollback transaction on error
            $this->orderModel->db->rollBack();
            
            return [
                'success' => false,
                'message' => $e->getMessage()
            ];
        }
    }
    
    // Additional methods...
}
```

### Security Implementation

Security is implemented through multiple layers:

**Authentication:**
- Secure password hashing with PHP's password_hash()
- Remember me functionality with secure tokens
- Session management with proper regeneration
- Rate limiting for login attempts

**CSRF Protection:**
```php
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
    
    // Additional methods...
}
```

**Data Validation:**
- Input filtering and sanitization
- Type checking and constraint validation
- Prepared statements for all database queries

**Headers and Cookies:**
- Secure and HTTP-only cookies
- Content Security Policy implementation
- X-Frame-Options and XSS protection headers

---

## 🚀 Performance Optimizations

### Caching Strategy

The platform implements a multi-layered caching approach:

**Database Query Caching:**
```php
public function getProductsByCategory($categoryId, $useCache = true) {
    $cacheKey = "products_category_{$categoryId}";
    
    if ($useCache && $this->cache->has($cacheKey)) {
        return $this->cache->get($cacheKey);
    }
    
    $result = $this->db->query("SELECT * FROM products WHERE category_id = :category_id AND is_active = 1")
                       ->bind(':category_id', $categoryId)
                       ->resultSet();
    
    if ($useCache) {
        $this->cache->set($cacheKey, $result, 3600); // Cache for 1 hour
    }
    
    return $result;
}
```

**Content Caching:**
- Full page caching for static pages
- Fragment caching for reusable components
- ESI (Edge Side Includes) for personalized content

**Asset Optimization:**
- Browser caching with appropriate headers
- Cache busting for assets with content hashes
- CDN integration for global availability

### Database Optimization

Database performance is maximized through:

- Proper indexing strategy
- Query optimization and profiling
- Connection pooling
- Denormalization where appropriate
- Pagination for large datasets

### Load Testing and Scaling

The platform is designed to scale efficiently:

- Horizontal scaling capabilities
- Stateless application design
- Database read replicas support
- Queue system for background processing
- API rate limiting

---

## 🔧 Development Workflow

### Local Development Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/lumiere.git
cd lumiere

# Install PHP dependencies
composer install

# Install frontend dependencies
npm install

# Set up environment
cp .env.example .env
php artisan key:generate

# Set up database
php artisan migrate --seed

# Compile assets
npm run dev

# Start the development server
php artisan serve
```

### Development Standards

- PSR-12 coding standards
- Semantic versioning
- Comprehensive documentation
- Test-driven development

### Git Workflow

- Feature branch workflow
- Pull request reviews
- Continuous integration checks
- Semantic commit messages

---

## 📊 Analytics and Monitoring

The platform includes comprehensive analytics and monitoring capabilities:

- Server-side performance metrics
- Real-time user behavior tracking
- Conversion funnel analysis
- A/B testing framework
- Error logging and alerting

---

## 🔮 Future Roadmap

### Phase 1: Core Enhancements (Q3 2025)
- Product recommendation engine using machine learning
- Multi-language support
- Enhanced search with faceted navigation
- Integration with additional payment gateways

### Phase 2: Omni-channel Capabilities (Q4 2025)
- Mobile app development
- In-store integration options
- Subscription model implementation
- Loyalty program

### Phase 3: Advanced Personalization (Q1 2026)
- Customer-specific pricing
- AI-driven content personalization
- Virtual product try-on
- Advanced analytics dashboard

---

## 👥 Contributing

We welcome contributions to the LUMIÈRE platform! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details on:

- Code of conduct
- Issue reporting guidelines
- Pull request process
- Development setup
- Testing requirements

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

LUMIÈRE was created by a dedicated team of designers and developers passionate about creating exceptional digital experiences for luxury brands. Special thanks to:

- [Design Team Members]
- [Development Team Members]
- [Open Source Contributors]

We also acknowledge the following technologies and tools that made this project possible:

- PHP and the Laravel framework
- MariaDB
- Tailwind CSS
- Alpine.js
- GSAP
- And the many other open-source packages listed in our dependencies

---

<p align="center">
  <strong>LUMIÈRE</strong><br>
  <i>Elevating the art of digital commerce for luxury aromatherapy.</i>
</p>
