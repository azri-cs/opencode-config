---
description: Analyze performance issues and provide optimization recommendations
agent: performance-auditor
temperature: 0.2
subtask: true
---

Perform comprehensive performance analysis and optimization recommendations. Use the relevant stack skill when framework or runtime details change the recommended optimization path.

**Performance Analysis:**
!`find . -type f \( -name "*.php" -o -name "*.py" -o -name "*.js" -o -name "*.ts" \) ! -path "./vendor/*" ! -path "./node_modules/*" ! -path "./.git/*" | wc -l`

!`du -sh . 2>/dev/null | head -1`

!`du -sh node_modules vendor 2>/dev/null | head -2`

## Performance Analysis Report

### Summary
- **Total Files**: [count]
- **Project Size**: [size]
- **Dependencies**: [count]
- **Overall Performance Score**: [X/100]

### Key Metrics
| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| First Contentful Paint | [X ms] | < 1.8s | [Pass/Fail] |
| Largest Contentful Paint | [X ms] | < 2.5s | [Pass/Fail] |
| Time to Interactive | [X ms] | < 3.8s | [Pass/Fail] |
| Cumulative Layout Shift | [X] | < 0.1 | [Pass/Fail] |
| Bundle Size | [X MB] | < 500KB | [Pass/Fail] |
| API Response Time (p95) | [X ms] | < 500ms | [Pass/Fail] |
| Database Query Time (avg) | [X ms] | < 100ms | [Pass/Fail] |

---

## Critical Performance Issues (Must Fix)

### 1. Bundle Size Optimization

**Issue**: Large JavaScript bundle impacting load time

**Current Analysis**:
- **Total Bundle Size**: [X MB]
- **Largest Files**:
  - [file:size] - [reason]
  - [file:size] - [reason]
  - [file:size] - [reason]
- **Duplicate Dependencies**: [count] packages

**Recommendations**:

#### Code Splitting
```javascript
// Instead of single large bundle
import { Component } from './large-component';

// Use dynamic imports
const Component = lazy(() => import('./large-component'));

// Route-based splitting
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Settings = lazy(() => import('./pages/Settings'));
const Reports = lazy(() => import('./pages/Reports'));
```

#### Tree Shaking
```javascript
// Wrong (imports entire library)
import _ from 'lodash';
_.debounce(fn, 300);

// Right (import specific function)
import debounce from 'lodash/debounce';
```

#### Remove Unused Code
```bash
# Find unused exports
npx webpack-bundle-analyzer

# Remove unused dependencies
npm uninstall unused-package
```

#### Compress Assets
```javascript
// webpack.config.js
module.exports = {
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({
        terserOptions: {
          compress: {
            drop_console: true,
            drop_debugger: true,
          },
        },
      }),
    ],
  },
};
```

### 2. Database Query Optimization

**Issue**: Slow database queries causing performance bottlenecks

**Query Analysis**:
- **Slow Queries**: [count]
- **N+1 Problems**: [count]
- **Missing Indexes**: [count]

**Examples**:

#### N+1 Query Problem
```php
// Wrong (N+1 queries)
$users = User::all();
foreach ($users as $user) {
    echo $user->posts->count(); // Each iteration queries posts
}

// Right (eager loading)
$users = User::with('posts')->get();
foreach ($users as $user) {
    echo $user->posts->count(); // No additional queries
}
```

```python
# Wrong (N+1 queries)
users = User.objects.all()
for user in users:
    posts = Post.objects.filter(user=user)  # Query per user

# Right (prefetch)
from django.db.models import Prefetch
users = User.objects.prefetch_related('post_set')
for user in users:
    posts = user.post_set.all()  # No additional query
```

#### Missing Indexes
```sql
-- Wrong (full table scan)
SELECT * FROM orders WHERE DATE(created_at) = '2024-01-01';

-- Right (index on created_at)
CREATE INDEX idx_orders_created_at ON orders(created_at);

-- Right (use index)
SELECT * FROM orders 
WHERE created_at >= '2024-01-01 00:00:00' 
AND created_at < '2024-01-02 00:00:00';
```

#### Slow Query Optimization
```php
// Wrong (complex query with subquery)
$orders = Order::whereIn('user_id', 
    User::where('active', true)->pluck('id')
)->get();

// Right (join query)
$orders = Order::whereHas('user', function($query) {
    $query->where('active', true);
})->get();
```

### 3. API Response Time

**Issue**: Slow API endpoints

**Analysis**:
- **Slowest Endpoints**:
  - [endpoint:path] - [X ms] - [reason]
  - [endpoint:path] - [X ms] - [reason]
- **Bottlenecks**:
  - Database queries: [X%]
  - External API calls: [X%]
  - Processing: [X%]

**Optimization Strategies**:

#### Caching
```php
// Laravel - Cache query results
use Illuminate\Support\Facades\Cache;

public function getUsers()
{
    return Cache::remember('users.active', 3600, function () {
        return User::where('active', true)->get();
    });
}
```

```python
# Django - Cache with Redis
from django.core.cache import cache

def get_users():
    users = cache.get('users:active')
    if users is None:
        users = User.objects.filter(is_active=True)
        cache.set('users:active', users, 3600)
    return users
```

#### Pagination
```javascript
// Wrong (all records)
const users = await User.find({});

// Right (paginated)
const users = await User.find({})
  .skip(offset)
  .limit(limit);
```

#### Rate Limiting
```php
// Laravel - Throttle API calls
Route::middleware(['throttle:60,1'])->group(function () {
    Route::get('/api/data', [DataController::class, 'index']);
});
```

### 4. Memory Leaks

**Issue**: Memory usage growing over time

**Analysis**:
- **Memory Growth Rate**: [X MB/hour]
- **Leak Locations**:
  - [file:line] - [issue]
  - [file:line] - [issue]

**Examples**:

#### Event Listener Leaks
```javascript
// Wrong (memory leak)
function Component() {
  useEffect(() => {
    const handler = () => console.log('resize');
    window.addEventListener('resize', handler);
    
    // Missing cleanup!
  }, []);
}

// Right (with cleanup)
function Component() {
  useEffect(() => {
    const handler = () => console.log('resize');
    window.addEventListener('resize', handler);
    
    return () => {
      window.removeEventListener('resize', handler);
    };
  }, []);
}
```

#### Subscription Leaks
```typescript
// Wrong (subscription leak)
function useData() {
  const [data, setData] = useState([]);
  
  useEffect(() => {
    const subscription = dataService.subscribe(setData);
    // Missing cleanup!
  }, []);
}

// Right (with cleanup)
function useData() {
  const [data, setData] = useState([]);
  
  useEffect(() => {
    const subscription = dataService.subscribe(setData);
    
    return () => {
      subscription.unsubscribe();
    };
  }, []);
}
```

---

## Serious Performance Issues (Should Fix)

### 1. Image Optimization

**Issue**: Large unoptimized images

**Analysis**:
- **Total Images**: [count]
- **Unoptimized**: [count]
- **Total Size**: [X MB]

**Solutions**:

```javascript
// Next.js Image optimization
import Image from 'next/image';

<Image
  src="/photo.jpg"
  alt="Description"
  width={800}
  height={600}
  loading="lazy"
  placeholder="blur"
  blurDataURL="data:image/jpeg;base64,..."
/>

// Or use responsive images
<img
  src="photo-800.jpg"
  srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1200.jpg 1200w"
  sizes="(max-width: 400px) 400px, (max-width: 800px) 800px, 1200px"
  alt="Description"
/>
```

### 2. CSS/JS Optimization

**Issue**: Large unoptimized CSS and JS

**Solutions**:

#### Remove Unused CSS
```bash
# Purge unused CSS
npm install -D purgecss
npx purgecss --config purgecss.config.js
```

#### Inline Critical CSS
```html
<!-- Critical CSS inline -->
<style>
  /* Only above-the-fold styles */
  .header { display: flex; }
  .hero { min-height: 100vh; }
</style>

<!-- Non-critical CSS async -->
<link rel="preload" href="styles.css" as="style" 
      onload="this.onload=null;this.rel='stylesheet'">
<noscript>
  <link rel="stylesheet" href="styles.css">
</noscript>
```

### 3. Database Connection Pooling

**Issue**: Connection overhead

**Configuration**:

```php
// Laravel - Optimize database connections
// config/database.php
'connections' => [
    'mysql' => [
        'driver' => 'mysql',
        'host' => env('DB_HOST', '127.0.0.1'),
        'port' => env('DB_PORT', '3306'),
        'database' => env('DB_DATABASE', 'forge'),
        'username' => env('DB_USERNAME', 'forge'),
        'password' => env('DB_PASSWORD', ''),
        'unix_socket' => env('DB_SOCKET', ''),
        'charset' => 'utf8mb4',
        'collation' => 'utf8mb4_unicode_ci',
        'prefix' => '',
        'prefix_indexes' => true,
        'strict' => true,
        'engine' => null,
        'options' => extension_loaded('pdo_mysql') ? array_filter([
            PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
        ]) : [],
        'pool' => [
            'min_connections' => 5,
            'max_connections' => 20,
            'wait_for_connection_timeout' => 60,
        ],
    ],
];
```

### 4. Lazy Loading

**Issue**: Loading resources eagerly

**Solutions**:

```javascript
// Lazy load components
const HeavyComponent = lazy(() => import('./HeavyComponent'));

// Lazy load images
<img loading="lazy" src="image.jpg" alt="Description" />

// Lazy load routes
const routes = [
  { path: '/', component: Home },
  { path: '/dashboard', component: Dashboard, lazy: true },
];

// Prefetch important resources
<link rel="prefetch" href="/dashboard.js" as="script">
```

---

## Performance Optimization Tools

### Profiling Tools
```bash
# Laravel
php artisan optimize:clear
php artisan route:cache
php artisan config:cache

# Django
python manage.py optimize

# Node.js
node --inspect index.js
```

### Monitoring Tools
- **Laravel**: Telescope, Clockwork
- **Django**: Django Debug Toolbar
- **React**: React DevTools Profiler
- **General**: New Relic, Datadog

---

## Performance Budget

### Recommended Budget
| Metric | Budget | Current | Status |
|--------|--------|---------|--------|
| JavaScript | < 500KB | [X KB] | [Pass/Fail] |
| CSS | < 100KB | [X KB] | [Pass/Fail] |
| Images | < 1MB | [X MB] | [Pass/Fail] |
| HTTP Requests | < 50 | [X] | [Pass/Fail] |
| API Response (p95) | < 500ms | [X ms] | [Pass/Fail] |
| Database Queries (avg) | < 100ms | [X ms] | [Pass/Fail] |

---

## Implementation Plan

### Phase 1: Immediate Wins (Week 1)
- [ ] Implement code splitting
- [ ] Add lazy loading for images
- [ ] Fix N+1 queries
- [ ] Add database indexes
- [ ] Enable compression

### Phase 2: Caching Layer (Week 2)
- [ ] Set up Redis cache
- [ ] Cache API responses
- [ ] Implement query caching
- [ ] Add CDN for static assets
- [ ] Configure browser caching

### Phase 3: Database Optimization (Week 3)
- [ ] Optimize slow queries
- [ ] Implement connection pooling
- [ ] Add query caching
- [ ] Optimize indexes
- [ ] Implement read replicas

### Phase 4: Advanced Optimization (Month 1)
- [ ] Implement service workers
- [ ] Add critical CSS inlining
- [ ] Optimize images further
- [ ] Implement preloading
- [ ] Fine-tune performance budgets

---

## Monitoring & Alerts

### Key Metrics to Monitor
- API response times (p50, p95, p99)
- Database query times
- Memory usage
- Error rates
- Cache hit ratios

### Alert Thresholds
- API response > 1s: Warning
- API response > 2s: Critical
- Memory > 80%: Warning
- Memory > 90%: Critical
- Error rate > 1%: Warning
