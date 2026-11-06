# 29 — Logging, Monitoring & Observability
**Project:** Modern Guideways corporate website  
**Version:** 1.0  
**Status:** Draft for owner review  
**Last updated:** 2026-11-04  
**Owner:** To be assigned  


## 1. Signals to monitor
- Site uptime and HTTP status.
- Frontend JavaScript errors.
- Frame asset 404/decode failures.
- Animation loader duration and timeout rate.
- Enquiry endpoint success/failure and notification delivery.
- API latency and server errors.
- Database health and backup status if Supabase is used.

## 2. Structured logs
Log request ID, route, event code, status, duration and non-sensitive diagnostic fields. Do not log names, emails, phone numbers, enquiry messages, tokens or raw secrets. If correlation is needed, use an opaque enquiry ID.

## 3. Alerts
Alert on sustained site outage, spike in frame errors, enquiry delivery failures, repeated API 5xx, database failure and certificate/domain issues. Assign a human owner and escalation path.

## 4. Dashboards
Separate visitor experience metrics from enquiry/business metrics. Restrict enquiry-level reporting to authorized company personnel. Use aggregate metrics for general product/marketing review.

## 5. Operational review
Review alerts after launch daily during initial watch, then weekly. Tune noisy alerts and update runbooks after incidents.
