ingly_linked_lists
ADME: The Great "404-pocalypse" of March 2025

## Project Overview
This project documents the postmortem of a critical outage on our e-commerce platform, **Shop-o-Tron 3000**, where 75% of users faced sluggish performance and persistent "404: Store Not Found" errors. The incident was caused by a misconfigured Nginx reverse proxy during deployment, resulting in a widespread service disruption.

## Issue Summary
**Duration:** March 1, 2025, 14:00 - 17:30 PST (3.5 hours of chaos)  
**Impact:** Customers were unable to browse or complete purchases, leading to a sharp rise in cart abandonment and an overwhelmed support team.  
**Root Cause:** A deployment error introduced a misconfigured Nginx reverse proxy, which mistakenly routed traffic to a non-existent backend server.

---

# Timeline
- **14:00 PST:** Monitoring detects backend latency spike; alert triggered.
- **14:05 PST:** Engineer notices the issue, and support receives a flood of user complaints.
- **14:15 PST:** Investigation focuses on database health, but no anomalies found.
- **14:45 PST:** Load balancer debugging leads to a dead end.
- **15:30 PST:** DevOps team takes over and reviews Nginx configurations.
- **16:45 PST:** The typo in the Nginx config is identified and fixed.
- **17:30 PST:** Services restored, and normal operations resume.

---

## Root Cause & Resolution
### Root Cause
During a routine deployment, an incorrect Nginx configuration pointed the reverse proxy to `app_server_9000` instead of `app_server_8000`, effectively cutting off access to the backend servers. The result was a flood of "404 Not Found" errors across the site.

### Resolution
- Reverted to the last known working Nginx configuration.
- Restarted Nginx (`systemctl restart nginx`) to apply the corrected settings.
- Verified fix with test transactions to confirm full functionality.

---

## Corrective Actions & Preventative Measures
### What Went Wrong
- No automated validation of Nginx configurations pre-deployment.
- Debugging efforts wasted time on unrelated components.

### How We're Fixing It
- [ ] Implement a syntax check for Nginx configs before deployment (`nginx -t`).
- [ ] Automate backend health checks with a script (`curl -I http://backend:8000/health`).
- [ ] Enhance monitoring for proxy-to-backend latency (Prometheus metric: `nginx_backend_response_time`).
- [ ] Require peer review for all configuration changes.
- [ ] Conduct a team workshop: "How Not to Break Everything."

---

## Why This Matters
This incident serves as a reminder that even a small typo can cause major outages. The goal is to ensure this kind of failure never happens again through better validation, automation, and team collaboration.

![Diagram of the 404-pocalypse](Insert your diagram here – perhaps Nginx as a confused traffic cop misdirecting traffic!)


