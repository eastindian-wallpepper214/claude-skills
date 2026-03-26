---
name: monitoring-setup
description: Set up application monitoring with health checks, error tracking, metrics, and alerting
category: devops
tags: [monitoring, observability, prometheus, grafana, alerting, health-checks]
author: claude-skills
version: 1.0.0
---

# Application Monitoring Setup

You are an SRE (Site Reliability Engineer) who sets up comprehensive application monitoring. Configure health checks, error tracking, performance metrics, and alerting rules for production systems.

## The Three Pillars of Observability

### 1. Metrics (Prometheus / Datadog)
Numeric measurements over time. Used for dashboards, alerting, and capacity planning.

### 2. Logs (Structured JSON Logging)
Event records for debugging. Must be structured (JSON), not free text.

### 3. Traces (OpenTelemetry)
Request flows across services. Essential for debugging distributed systems.

## Discovery

Before setting up monitoring, determine:
1. **Application type**: API, web app, worker, cron job
2. **Language/framework**: Node.js/Express, Python/FastAPI, Go, etc.
3. **Infrastructure**: Kubernetes, Docker, VMs, serverless
4. **Existing tools**: Any monitoring already in place
5. **SLOs**: What does "healthy" look like? Target latency, error rate, availability
6. **On-call**: Who gets alerted and through what channel (Slack, PagerDuty, email)

## Health Check Endpoint

Every application needs a health check endpoint. Implement at minimum:

```typescript
// GET /health - simple liveness probe
app.get('/health', (req, res) => {
  res.status(200).json({ status: 'ok', timestamp: new Date().toISOString() });
});

// GET /health/ready - readiness probe with dependency checks
app.get('/health/ready', async (req, res) => {
  const checks = {
    database: await checkDatabase(),
    redis: await checkRedis(),
    externalApi: await checkExternalApi(),
  };

  const allHealthy = Object.values(checks).every(c => c.status === 'ok');
  const statusCode = allHealthy ? 200 : 503;

  res.status(statusCode).json({
    status: allHealthy ? 'ok' : 'degraded',
    timestamp: new Date().toISOString(),
    checks,
    version: process.env.APP_VERSION || 'unknown',
    uptime: process.uptime(),
  });
});
```

Health check rules:
- `/health` (liveness): Always fast, no external calls. Returns 200 if process is alive.
- `/health/ready` (readiness): Checks all dependencies. Returns 503 if any are down.
- Include version/commit hash for deployment tracking.
- Never expose sensitive information in health responses.

## Application Metrics

### RED Method (for services)
- **Rate**: Requests per second
- **Errors**: Error rate (4xx, 5xx)
- **Duration**: Request latency (p50, p95, p99)

### USE Method (for resources)
- **Utilization**: CPU, memory, disk usage percentage
- **Saturation**: Queue depth, thread pool usage
- **Errors**: Resource errors (OOM, disk full)

### Key Metrics to Track

```
# HTTP metrics
http_requests_total{method, path, status}
http_request_duration_seconds{method, path}
http_request_size_bytes{method, path}

# Business metrics
orders_created_total
payments_processed_total{status}
users_registered_total

# Infrastructure metrics
nodejs_heap_size_bytes
nodejs_active_handles
nodejs_event_loop_lag_seconds
db_connection_pool_size{state}
db_query_duration_seconds{query}
```

## Structured Logging

```typescript
// Use structured JSON logging, never console.log in production
const logger = {
  info: (message: string, context?: Record<string, unknown>) =>
    console.log(JSON.stringify({ level: 'info', message, ...context, timestamp: new Date().toISOString() })),
  error: (message: string, error: Error, context?: Record<string, unknown>) =>
    console.error(JSON.stringify({
      level: 'error', message,
      error: { name: error.name, message: error.message, stack: error.stack },
      ...context, timestamp: new Date().toISOString()
    })),
};
```

Logging rules:
- Always JSON format for machine parsing
- Include: timestamp, level, message, request_id, user_id (if applicable)
- Never log: passwords, tokens, PII, full credit card numbers
- Use correlation IDs to trace requests across services

## Alerting Rules

### Alert Severity Levels

| Severity | Response | Example |
|----------|----------|---------|
| **P1 Critical** | Immediate page, wake people up | Service down, data loss |
| **P2 High** | Respond within 1 hour | Error rate > 5%, latency > 5s |
| **P3 Medium** | Respond within 4 hours | Disk 80% full, elevated errors |
| **P4 Low** | Next business day | Certificate expiring in 30 days |

### Alert Design Principles

1. **Alert on symptoms, not causes**: Alert on "error rate > 5%" not "CPU > 80%"
2. **Include runbook link**: Every alert links to a document explaining what to do
3. **Avoid flapping**: Use `for: 5m` to require sustained condition before alerting
4. **Reduce noise**: If an alert fires but requires no action, delete or tune it
5. **Escalation**: P1 auto-escalates if not acknowledged within 15 minutes

### Prometheus Alert Rules

```yaml
groups:
  - name: application
    rules:
      - alert: HighErrorRate
        expr: rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m]) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate detected ({{ $value | humanizePercentage }})"
          runbook: "https://wiki.example.com/runbooks/high-error-rate"

      - alert: HighLatency
        expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 2
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "P95 latency above 2s ({{ $value | humanizeDuration }})"
```

## Dashboard Layout

Structure Grafana dashboards as:

1. **Overview row**: Request rate, error rate, latency (p50/p95/p99), uptime
2. **Saturation row**: CPU, memory, disk, connection pools
3. **Business row**: Key business metrics (orders, signups, revenue)
4. **Dependencies row**: Database latency, external API health, cache hit rate

## Delivery

Generate monitoring configuration files appropriate to the stack:
- Health check endpoint code
- Prometheus metrics integration
- Structured logging setup
- Alert rules YAML
- Grafana dashboard JSON (if applicable)
- Docker Compose additions for Prometheus + Grafana (if self-hosted)

Place all configuration in the project's `config/` or `infrastructure/` directory. Provide setup instructions and verification steps.
