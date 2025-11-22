---
name: performance-optimizer
description: This skill should be used when the user requests performance analysis, optimization recommendations, bottleneck identification, load time improvements, or resource usage optimization. It provides comprehensive performance profiling and optimization strategies for web applications, APIs, and databases.
---

# Performance Optimizer

## Overview

This skill enables comprehensive performance analysis and optimization for web applications, APIs, and databases. It identifies bottlenecks, analyzes resource usage, and provides actionable optimization strategies. The skill covers frontend performance, backend optimization, database tuning, and infrastructure scaling.

## When to Use This Skill

Use this skill when the user requests:
- Performance analysis or profiling
- Load time optimization
- Database query optimization
- API response time improvement
- Memory leak detection
- Resource usage optimization
- Caching strategy implementation
- Frontend performance tuning
- Backend optimization
- Infrastructure scaling recommendations

## Core Workflow

### Step 1: Performance Assessment

**Frontend Performance**:
- Page load time analysis
- Core Web Vitals (LCP, FID, CLS)
- JavaScript bundle size
- Image optimization
- CSS optimization
- Network waterfall analysis

**Backend Performance**:
- API response times
- Database query performance
- Memory usage
- CPU utilization
- I/O operations
- Request throughput

**Database Performance**:
- Slow query identification
- Index optimization
- Query execution plans
- Connection pooling
- Cache hit rates

### Step 2: Bottleneck Identification

**Common Bottlenecks**:
- N+1 query problems
- Missing database indexes
- Inefficient algorithms
- Memory leaks
- Blocking operations
- Large payload sizes
- Unoptimized images
- Too many HTTP requests

### Step 3: Optimization Strategies

**Frontend Optimization**:
```javascript
// Code splitting
const LazyComponent = React.lazy(() => import('./LazyComponent'));

// Image optimization
<img
  src="image.webp"
  loading="lazy"
  width="800"
  height="600"
  alt="Description"
/>

// Resource hints
<link rel="preconnect" href="https://api.example.com">
<link rel="dns-prefetch" href="https://cdn.example.com">
```

**Backend Optimization**:
```javascript
// Caching
const cache = new NodeCache({ stdTTL: 600 });

app.get('/api/users', async (req, res) => {
  const cacheKey = 'users_list';
  let users = cache.get(cacheKey);

  if (!users) {
    users = await User.findAll();
    cache.set(cacheKey, users);
  }

  res.json(users);
});

// Connection pooling
const pool = new Pool({
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000
});
```

**Database Optimization**:
```sql
-- Add indexes
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_created_at ON orders(created_at);

-- Query optimization
-- Before (N+1 problem)
SELECT * FROM users;
-- Then for each user: SELECT * FROM orders WHERE user_id = ?

-- After (Eager loading)
SELECT u.*, o.*
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

### Step 4: Performance Metrics

**Core Web Vitals**:
- **LCP** (Largest Contentful Paint): < 2.5s
- **FID** (First Input Delay): < 100ms
- **CLS** (Cumulative Layout Shift): < 0.1

**API Performance**:
- Response time: < 200ms (p95)
- Throughput: Requests per second
- Error rate: < 0.1%

**Database Performance**:
- Query execution time: < 50ms
- Cache hit rate: > 80%
- Connection pool usage: < 70%

### Step 5: Generate Performance Report

Report includes:
1. Current performance metrics
2. Identified bottlenecks
3. Optimization recommendations (prioritized)
4. Expected improvements
5. Implementation effort
6. Before/after comparisons

## Best Practices

**Frontend**:
- Minimize and compress assets
- Use CDN for static resources
- Implement lazy loading
- Optimize images (WebP, compression)
- Reduce JavaScript bundle size
- Use service workers for caching

**Backend**:
- Implement caching layers
- Use connection pooling
- Optimize database queries
- Implement rate limiting
- Use async/await properly
- Profile and monitor continuously

**Database**:
- Add appropriate indexes
- Avoid SELECT *
- Use query result caching
- Implement database connection pooling
- Optimize table structures
- Regular maintenance (VACUUM, ANALYZE)

## Performance Testing Tools

**Frontend**:
- Lighthouse
- WebPageTest
- Chrome DevTools Performance
- webpack-bundle-analyzer

**Backend**:
- New Relic, DataDog
- Apache Bench (ab)
- Artillery, k6
- Node.js profiler

**Database**:
- EXPLAIN ANALYZE
- pg_stat_statements
- MySQL Slow Query Log
- Database-specific profilers
