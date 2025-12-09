# Operations & Production Concerns

Running systems reliably in production.

---

## Monitoring & Observability

Understanding system behavior through data.

### The Three Pillars

**1. Metrics**
Numeric measurements over time.
- **Golden Signals:** Latency, traffic, errors, saturation
- **USE Method:** Utilization, saturation, errors (for resources)
- **RED Method:** Rate, errors, duration (for services)
- **Tools:** Prometheus, Datadog, CloudWatch

**2. Logs**
Structured or unstructured event records.
- **Levels:** DEBUG, INFO, WARN, ERROR
- **Structure:** Use JSON for easy parsing
- **Correlation:** Include trace/request IDs
- **Tools:** ELK Stack, Splunk, Loki

**3. Tracing**
Track requests across distributed services.
- **Distributed tracing:** Follow request through multiple services
- **Spans:** Individual operations
- **Sampling:** Trace subset of requests to reduce overhead
- **Tools:** Jaeger, Zipkin, OpenTelemetry

---

## Alerting

Notify on-call engineers of problems.

### Alert Design
- **Actionable:** Clear what to do
- **Specific:** Identify affected component
- **Timely:** Before users notice
- **Prioritized:** Critical vs warning

### Avoid Alert Fatigue
- Don't alert on symptoms you can't fix
- Use appropriate thresholds
- Aggregate related alerts
- Auto-resolve when issue clears

### On-Call Best Practices
- Runbooks for common issues
- Escalation policies
- Blameless post-mortems
- On-call compensation

---

## Deployment Strategies

Release changes safely.

### Blue-Green Deployment
- Run two identical environments (blue = current, green = new)
- Switch traffic to green, keep blue as rollback
- **Pros:** Instant rollback, test in prod-like environment
- **Cons:** 2x resources, database migrations tricky

### Canary Deployment
- Deploy to small subset of servers/users first
- Monitor metrics, gradually increase traffic
- **Pros:** Catch issues early, limited blast radius
- **Cons:** Requires metrics and automation

### Rolling Update
- Replace instances gradually (25% at a time)
- **Pros:** No extra resources needed
- **Cons:** Mixed versions running simultaneously

### Feature Flags
- Deploy code disabled, enable per user/environment
- **Pros:** Decouple deploy from release, quick rollback
- **Cons:** Code complexity, flag cleanup debt

---

## Security

Protect systems and data.

### Authentication
Verify identity of users/services.
- **Methods:** Passwords, OAuth, SAML, mTLS
- **Best practices:** MFA, password hashing (bcrypt), token rotation

### Authorization
Control what authenticated users can do.
- **RBAC:** Role-Based Access Control
- **ABAC:** Attribute-Based Access Control
- **Principle of least privilege:** Grant minimal necessary permissions

### Encryption
- **In transit:** TLS/SSL for all network communication
- **At rest:** Encrypt disks, databases, backups
- **Key management:** Use KMS (Key Management Service)

### Secrets Management
- Never commit secrets to code
- Use secret stores (Vault, AWS Secrets Manager)
- Rotate regularly
- Audit access

---

## Disaster Recovery

Prepare for catastrophic failures.

### Backup Strategy
- **3-2-1 Rule:** 3 copies, 2 different media, 1 offsite
- **Regular testing:** Restore from backups periodically
- **Automation:** Automated backup schedules

### Key Metrics

**RTO (Recovery Time Objective)**
How long system can be down.
- **Example:** "System must be restored within 4 hours"

**RPO (Recovery Point Objective)**
How much data loss is acceptable.
- **Example:** "Can lose up to 1 hour of data"

### Multi-Region
- **Active-Active:** Traffic to multiple regions
- **Active-Passive:** Failover to backup region
- **Data replication:** Async or sync cross-region

---

## Capacity Planning

Ensure system can handle expected load.

**Process:**
1. Measure current usage and growth rate
2. Project future demand
3. Identify bottlenecks
4. Plan infrastructure scaling
5. Load test before peak events

**Headroom:** Maintain 20-30% spare capacity for spikes

---

## Cost Optimization

- **Right-size instances:** Don't overprovision
- **Auto-scaling:** Scale down during low traffic
- **Reserved instances:** Commit for discount
- **Spot instances:** For fault-tolerant workloads
- **Delete unused resources:** Old snapshots, test environments
- **Monitor and alert on cost:** Prevent surprises

---

## Operational Checklist

Production readiness:
- [ ] Monitoring and dashboards
- [ ] Alerts with runbooks
- [ ] Deployment automation (CI/CD)
- [ ] Backup and restore tested
- [ ] Security review (encryption, secrets, access)
- [ ] Load testing completed
- [ ] Disaster recovery plan
- [ ] On-call rotation and escalation
- [ ] Documentation (architecture, APIs, runbooks)
- [ ] Cost monitoring and budgets

---

## Incident Response

1. **Detect:** Alerts, monitoring, user reports
2. **Triage:** Assess severity and impact
3. **Mitigate:** Quick fix to restore service
4. **Communicate:** Update status page, notify stakeholders
5. **Resolve:** Permanent fix
6. **Post-mortem:** Blameless review, action items

---

## References

- [Google SRE Book](https://sre.google/sre-book/table-of-contents/)
- *Site Reliability Engineering* by Beyer et al.
- [AWS Well-Architected: Operational Excellence](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/)
- [The Phoenix Project](https://itrevolution.com/the-phoenix-project/) by Gene Kim

