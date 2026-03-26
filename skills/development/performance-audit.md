---
name: performance-audit
description: Performance audit for N+1 queries, memory leaks, blocking ops, and bundle size issues
category: development
tags: [performance, optimization, profiling, memory, bundle-size]
author: claude-skills
version: 1.0.0
---

# Performance Audit

Analyze the codebase for performance issues. Identify bottlenecks, quantify impact, and provide targeted fixes.

## Execution Steps

### 1. Determine the Stack

Read package.json, config files, and entry points to identify:
- Runtime (Node.js, browser, Deno, Bun)
- Framework (Express, Next.js, React, Vue, etc.)
- Database (PostgreSQL, MongoDB, Redis, etc.)
- Build tool (webpack, vite, esbuild, turbopack)

This determines which categories of performance issues to prioritize.

### 2. Database and Query Performance

Search for:
- **N+1 queries**: Loops that execute individual DB calls. Look for `await` inside `for`/`forEach`/`map` where the call hits a database or API.
  - Fix: Use batch queries (`WHERE id IN (...)`) or ORM eager loading (`include`, `populate`, `joinRelation`)
- **Missing pagination**: `find()` or `SELECT *` without `LIMIT`
  - Fix: Always paginate collection queries
- **Unbounded joins**: Queries joining multiple tables without filters
  - Fix: Add WHERE clauses, select only needed columns
- **Missing indexes**: Queries filtering/sorting on columns not in migration index definitions
  - Fix: Add database indexes for frequently queried columns
- **Unnecessary SELECT**: Fetching all columns when only a few are needed
  - Fix: Use `.select()` or explicit column lists

### 3. Memory Leaks and Allocation

Check for:
- **Event listener leaks**: Adding listeners in loops or component mounts without removal
  - Fix: Always remove listeners in cleanup (useEffect return, removeEventListener, off())
- **Growing collections**: Maps, Sets, or arrays that grow without eviction
  - Fix: Use LRU cache with max size, or WeakMap/WeakRef for object caches
- **Closure captures**: Functions in hot paths that capture large scopes
  - Fix: Extract and minimize captured variables
- **Stream backpressure**: Readable streams piped to slower writable streams without backpressure handling
  - Fix: Use pipeline() or check writable.write() return value
- **Unreleased resources**: Database connections, file handles, or HTTP agents not closed

### 4. Blocking Operations

Identify:
- **Synchronous file I/O**: `fs.readFileSync`, `fs.writeFileSync` in request handlers
  - Fix: Use async equivalents (`fs.promises.readFile`)
- **CPU-intensive work on main thread**: JSON.parse of large payloads, crypto operations, image processing
  - Fix: Offload to worker threads or a background job queue
- **Missing concurrency**: Sequential awaits that could be parallel
  - Fix: Use `Promise.all()` for independent async operations
- **Large synchronous iterations**: Processing thousands of items in a single tick
  - Fix: Batch processing with setImmediate/setTimeout breaks, or use streams

### 5. Frontend-Specific (if applicable)

Check for:
- **Unnecessary re-renders**: Components re-rendering when props have not changed
  - Fix: React.memo, useMemo, useCallback with correct dependency arrays
- **Large bundle size**: Importing entire libraries when only a function is needed (`import _ from 'lodash'` vs `import debounce from 'lodash/debounce'`)
  - Fix: Tree-shake, use specific imports, dynamic import() for code splitting
- **Render-blocking resources**: Large CSS/JS files in the critical path
  - Fix: Lazy load below-fold components, defer non-critical scripts
- **Unoptimized images**: Large images served without responsive sizing or modern formats
  - Fix: Use next/image, srcset, WebP/AVIF formats
- **Layout thrashing**: Reading layout properties (offsetHeight) then writing styles in a loop
  - Fix: Batch reads, then batch writes

### 6. Caching Opportunities

Identify:
- Expensive computations repeated with the same inputs (memoization candidate)
- Database queries for rarely-changing data (cache with TTL)
- API responses that could be cached (HTTP cache headers, CDN)
- Repeated parsing of the same config or template files

### 7. Output Format

```
## Performance Audit: [project/file]

### Critical (major impact on user experience or costs)
- [N+1] path/service.ts:XX — Loop executes N queries; batch with IN clause
  Impact: O(N) queries reduced to O(1)

### High (noticeable degradation under load)
- [BLOCKING] path/handler.ts:XX — Synchronous file read in request handler
  Impact: Blocks event loop for ~50ms per request

### Medium (optimization opportunity)
- [MEMORY] path/cache.ts:XX — Unbounded Map grows without eviction
  Impact: Memory usage increases linearly with traffic

### Low (minor improvement)
- [BUNDLE] Lodash fully imported, only debounce used
  Impact: ~70KB unnecessary bundle size

### Metrics Summary
- Issues found: N (X critical, Y high, Z medium, W low)
- Estimated impact: [description of expected improvement]
```

### 8. Apply Fixes

For each Critical and High finding, provide the exact code change. Apply using the Edit tool after confirmation. Re-run tests after each fix to verify no regressions.
