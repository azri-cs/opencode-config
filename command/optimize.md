---
description: Optimize performance of specified component or feature
agent: performance-auditor
temperature: 0.2
subtask: true
---

Analyze and optimize performance of the component or feature specified as $ARGUMENTS. Use the relevant stack skill when framework or runtime details affect the optimization strategy.

**Performance Analysis:**
!`find . -name "$ARGUMENTS" -o -name "$(echo $ARGUMENTS | sed 's/.*\///')" 2>/dev/null | head -5`

!`du -sh $ARGUMENTS 2>/dev/null || echo "Checking component size..."`

!`cat package.json 2>/dev/null | grep -E "dependencies|devDependencies" | head -5`

## Performance Optimization Plan

### Component Analysis
- **Component**: $ARGUMENTS
- **Type**: [API/Component/Database/Algorithm]
- **Current Performance**: [metrics]
- **Target Performance**: [metrics]
- **Priority**: [High/Medium/Low]

### Current Bottlenecks Identified

**Performance Metrics**:
| Metric | Current | Target | Improvement |
|--------|---------|--------|-------------|
| Response Time | [X ms] | < [Y ms] | [Z%] |
| Memory Usage | [X MB] | < [Y MB] | [Z%] |
| CPU Usage | [X%] | < [Y%] | [Z%] |
| Database Queries | [X] | < [Y] | [Z%] |
| Bundle Size | [X KB] | < [Y KB] | [Z%] |

---

## Optimization Strategy

### 1. Code-Level Optimizations

#### 1.1 Algorithm Optimization

**Current Implementation**:
```[language]
// Slow algorithm - O(n²)
function findDuplicates(items) {
  const duplicates = [];
  for (let i = 0; i < items.length; i++) {
    for (let j = i + 1; j < items.length; j++) {
      if (items[i] === items[j]) {
        duplicates.push(items[i]);
      }
    }
  }
  return [...new Set(duplicates)];
}

// Optimized - O(n)
function findDuplicates(items) {
  const seen = new Set();
  const duplicates = new Set();
  
  for (const item of items) {
    if (seen.has(item)) {
      duplicates.add(item);
    } else {
      seen.add(item);
    }
  }
  
  return Array.from(duplicates);
}
```

**Complexity Analysis**:
- **Before**: O(n²) - Quadratic time
- **After**: O(n) - Linear time
- **Speedup**: [X]x faster for n=1000

#### 1.2 Data Structure Optimization

**Current**:
```[language]
// Array search - O(n)
const users = getUsers();
const activeUser = users.find(u => u.active);

// Optimized with Map - O(1)
const usersMap = new Map(users.map(u => [u.id, u]));
const activeUser = usersMap.get(userId);
```

**Memory vs Speed Trade-off**:
- **Time**: O(n) → O(1)
- **Space**: O(n) → O(n) + overhead

### 2. Database Optimizations

#### 2.1 Query Optimization

**Before (N+1 Query Problem)**:
```php
// 1 + N queries
$posts = Post::all();
foreach ($posts as $post) {
    echo $post->author->name;  // Each iteration = 1 query
}
```

**After (Eager Loading)**:
```php
// 2 queries total
$posts = Post::with('author')->get();
foreach ($posts as $post) {
    echo $post->author->name;  // No additional queries
}
```

#### 2.2 Index Optimization

**Missing Index**:
```sql
-- Before: Full table scan
SELECT * FROM orders WHERE DATE(created_at) = '2024-01-01';

-- After: Index on created_at
CREATE INDEX idx_orders_created_at ON orders(created_at);

-- Query now uses index range scan
SELECT * FROM orders 
WHERE created_at >= '2024-01-01 00:00:00'
AND created_at < '2024-01-02 00:00:00';
```

#### 2.3 Caching Strategy

**Query Result Caching**:
```php
// Without cache
$users = User::where('active', true)->get();

// With cache
$users = Cache::remember('users.active', 3600, function () {
    return User::where('active', true)->get();
});
```

**Cache Invalidation**:
```php
// Invalidate when user is updated
class User extends Model
{
    protected static function booted()
    {
        static::updated(function ($user) {
            Cache::forget('user.' . $user->id);
            Cache::forget('users.active');
        });
    }
}
```

### 3. API Optimizations

#### 3.1 Response Optimization

**Before (Full Data)**:
```json
{
  "id": 1,
  "name": "John",
  "email": "john@example.com",
  "password_hash": "...",
  "api_token": "...",
  "created_at": "...",
  "updated_at": "..."
}
```

**After (DTO/Resource)**:
```json
{
  "id": 1,
  "name": "John"
}
```

**Selective Fields**:
```python
# Django REST Framework
class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'name', 'email']
        read_only_fields = ['id']
```

#### 3.2 Pagination

**Before (All Records)**:
```javascript
// Returns all 10,000 users
const users = await User.find({});
```

**After (Paginated)**:
```javascript
// Returns 20 users per page
const users = await User.find({})
  .skip(20 * (page - 1))
  .limit(20);
```

### 4. Frontend Optimizations

#### 4.1 Bundle Size Reduction

**Before (Full Library)**:
```javascript
import _ from 'lodash';
const sum = _.sum([1, 2, 3]);
```

**After (Tree Shaking)**:
```javascript
import sum from 'lodash-es/sum';
const total = sum([1, 2, 3]);
```

**Code Splitting**:
```javascript
// Lazy load heavy components
const HeavyChart = lazy(() => import('./HeavyChart'));

function Dashboard() {
  return (
    <Suspense fallback={<Loading />}>
      <HeavyChart data={data} />
    </Suspense>
  );
}
```

#### 4.2 Rendering Optimization

**Before (Unnecessary Re-renders)**:
```javascript
function Parent() {
  const [count, setCount] = useState(0);
  
  return <Child onClick={() => setCount(count + 1)} />;
}

function Child({ onClick }) {
  // Re-renders every time count changes
  return <button onClick={onClick}>Click</button>;
}
```

**After (useCallback)**:
```javascript
function Parent() {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, []);
  
  return <Child onClick={handleClick} />;
}

function Child({ onClick }) {
  // Only re-renders when onClick changes
  return <button onClick={onClick}>Click</button>;
}
```

---

## Implementation Plan

### Phase 1: Quick Wins (Day 1)

#### 1.1 Add Database Indexes
```sql
-- Identify slow queries
EXPLAIN ANALYZE SELECT * FROM orders WHERE status = 'pending';

-- Add indexes for frequently queried columns
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_created_at ON orders(created_at);
```

#### 1.2 Implement Caching
```php
// Add cache to frequently accessed data
public function getDashboardStats()
{
    return Cache::remember('dashboard.stats', 300, function () {
        return [
            'users' => User::count(),
            'orders' => Order::count(),
            'revenue' => Order::sum('amount'),
        ];
    });
}
```

#### 1.3 Enable Compression
```python
# Django - Enable GZIP
MIDDLEWARE = [
    'django.middleware.gzip.GZipMiddleware',
    # ... other middleware
]
```

### Phase 2: Core Optimizations (Day 2-3)

#### 2.1 Optimize Database Queries
```php
// Replace N+1 with eager loading
$posts = Post::with(['author', 'comments', 'tags'])
    ->where('status', 'published')
    ->orderBy('created_at', 'desc')
    ->paginate(20);
```

#### 2.2 Implement Query Optimization
```php
// Use select for specific fields
$users = User::select('id', 'name', 'email')
    ->where('active', true)
    ->get();

// Use chunk for large datasets
User::chunk(200, function ($users) {
    foreach ($users as $user) {
        // Process user
    }
});
```

#### 2.3 Optimize API Responses
```php
// Use resources for response formatting
return UserResource::collection($users)
    ->additional([
        'meta' => [
            'total' => $users->total(),
            'page' => $users->currentPage(),
        ]
    ]);
```

### Phase 3: Advanced Optimizations (Week 1)

#### 3.1 Implement Redis Caching
```php
// Redis cache configuration
config/database.php:
'redis' => [
    'client' => 'predis',
    'default' => [
        'host' => env('REDIS_HOST', '127.0.0.1'),
        'port' => env('REDIS_PORT', 6379),
        'database' => env('REDIS_DB', 0),
    ],
    'cache' => [
        'host' => env('REDIS_HOST', '127.0.0.1'),
        'port' => env('REDIS_PORT', 6379),
        'database' => env('REDIS_CACHE_DB', 1),
    ],
],
```

#### 3.2 Implement Queue for Heavy Tasks
```php
// Queue job for heavy processing
class ProcessOrders implements ShouldQueue
{
    public function handle()
    {
        // Process orders in background
    }
}

// Dispatch job
ProcessOrders::dispatch()->onQueue('high');
```

#### 3.3 Database Read Replicas
```php
// Configure read replica
'connections' => [
    'mysql' => [
        'write' => [
            'host' => 'primary.db.example.com',
        ],
        'read' => [
            ['host' => 'replica1.db.example.com'],
            ['host' => 'replica2.db.example.com'],
        ],
    ],
],
```

---

## Performance Metrics

### Before Optimization
| Metric | Value | Tool |
|--------|-------|------|
| Response Time (p95) | [X ms] | Apache Bench |
| Memory Usage | [X MB] | New Relic |
| Database Queries | [X/query] | Laravel Debugbar |
| Bundle Size | [X KB] | Webpack Bundle Analyzer |
| First Contentful Paint | [X ms] | Lighthouse |

### After Optimization (Target)
| Metric | Value | Target |
|--------|-------|--------|
| Response Time (p95) | [X ms] | < [Y ms] |
| Memory Usage | [X MB] | < [Y MB] |
| Database Queries | [X/query] | < [Y/query] |
| Bundle Size | [X KB] | < [Y KB] |
| First Contentful Paint | [X ms] | < [Y ms] |

---

## Testing Strategy

### Load Testing
```bash
# Apache Bench
ab -n 1000 -c 10 https://api.example.com/endpoint

# Artillery
artillery run load-test.yml

# Locust
locust -f locustfile.py --host=https://api.example.com
```

### Performance Testing
```javascript
// Lighthouse CI
const lighthouse = require('lighthouse');
const results = await lighthouse(url, options);
```

---

## Monitoring & Alerting

### Key Metrics to Monitor
- API response time (p50, p95, p99)
- Error rate
- Memory usage
- Database query time
- Cache hit ratio
- Queue job processing time

### Alert Thresholds
- API response > 1s: Warning
- API response > 2s: Critical
- Memory > 80%: Warning
- Error rate > 1%: Warning

### Dashboard
- Grafana dashboard for real-time monitoring
- Datadog/New Relic for APM
- Custom metrics for business logic

---

## Optimization Checklist

### Database
- [ ] Add missing indexes
- [ ] Optimize slow queries
- [ ] Implement query caching
- [ ] Use eager loading
- [ ] Implement read replicas

### API
- [ ] Add pagination
- [ ] Implement response compression
- [ ] Use efficient data formats
- [ ] Add rate limiting
- [ ] Implement caching headers

### Frontend
- [ ] Reduce bundle size
- [ ] Implement code splitting
- [ ] Optimize images
- [ ] Use lazy loading
- [ ] Optimize rendering

### Infrastructure
- [ ] Enable CDN
- [ ] Configure caching
- [ ] Set up monitoring
- [ ] Implement autoscaling
- [ ] Configure load balancing

---

## Documentation

### Update Documentation
- [ ] API documentation with performance notes
- [ ] README with optimization guidelines
- [ ] Configuration documentation
- [ ] Monitoring setup guide

### Performance Report
```
## Performance Optimization Report

### Summary
- Overall performance improvement: [X]%
- API response time reduction: [Y]%
- Memory usage reduction: [Z]%

### Changes Made
1. [Change 1]
   - Impact: [description]
   - Metric: [before] → [after]

2. [Change 2]
   - Impact: [description]
   - Metric: [before] → [after]

### Recommendations
1. [Recommendation 1]
2. [Recommendation 2]
```
