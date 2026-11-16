# ERP SRS v1.2 — AWS-first microservices architecture

## Logical microservices
```mermaid
flowchart TB
 U[React Web / Flutter / Admin] --> EDGE[WAF + API Gateway / BFF]
 EDGE --> IAM[Identity and Access / Keycloak]
 EDGE --> TEN[Tenant + Module Registry + Entitlements]
 EDGE --> CRM[CRM: Leads, Contacts, Deals, Activities]
 EDGE --> QUO[Quotation Service]
 EDGE --> PARTY[Canonical Party / Organisation]
 EDGE --> DOC[Documents]
 EDGE --> REPORT[Reporting / Read Models]
 CRM -->|REST: party lookup| PARTY
 QUO -->|REST: deal and party snapshot| CRM
 QUO -->|REST: canonical party| PARTY
 CRM --> OUT[Transactional Outbox / Event Bus]
 QUO --> OUT
 PARTY --> OUT
 OUT --> WORK[Automation / Workflow Workers]
 OUT --> REPORT
 OUT --> INT[Integration Hub / Adapters]
 INT --> EXT[WhatsApp / Telephony / Email / Lead Sources]
 WORK --> NOTIF[Notification Service]
 CRM --> DB1[(CRM PostgreSQL schema/database)]
 QUO --> DB2[(Quotation PostgreSQL schema/database)]
 PARTY --> DB3[(Party PostgreSQL schema/database)]
 TEN --> DB4[(Core PostgreSQL schema/database)]
 DOC --> OBJ[(Object Storage)]
 CRM --> CACHE[(Valkey / Redis cache)]
 EDGE --> OBS[OpenTelemetry]
 OUT --> AUDIT[Audit Log / Immutable Archive]
 FUT[Future Sales / Inventory / Finance / HRM / Projects / Support] -. APIs + events .-> OUT
```

## AWS infrastructure
```mermaid
flowchart TB
 USERS[Web / Mobile Users] --> DNS[Route 53 + CloudFront + WAF]
 DNS --> S3WEB[S3 static web assets]
 DNS --> APIGW[API Gateway / ALB]
 subgraph AWS[AWS Region — Production VPC, 3 AZ]
 APIGW --> ING[Private EKS ingress]
 subgraph EKS[EKS private worker nodes across AZs]
 ING --> CORE[Core services]
 ING --> CRM[CRM + Quotation + Party]
 ING --> INTEG[Integrations + Workflow workers]
 CORE --> OTEL[OpenTelemetry collectors]
 CRM --> OTEL
 INTEG --> OTEL
 end
 CORE --> RDS[(Aurora PostgreSQL / RDS Multi-AZ)]
 CRM --> RDS
 CRM --> VAL[(ElastiCache Valkey Multi-AZ)]
 INTEG --> BUS[EventBridge + SQS + DLQ]
 INTEG --> S3DOC[(S3 documents / audit / backups)]
 end
 BUS --> PROVIDERS[External approved providers]
 OTEL --> MON[CloudWatch / Managed Prometheus + Grafana]
 CICD[GitHub/GitLab CI + security scans] --> ECR[ECR image registry]
 ECR --> GITOPS[Argo CD + Helm]
 GITOPS --> EKS
 SECRETS[Secrets Manager + KMS] --> EKS
```

## Multi-cloud expansion (active-passive recommended first)
```mermaid
flowchart LR
 CLIENT[Clients] --> ROUTE[Global DNS / traffic policy]
 ROUTE -->|Primary| AWS[AWS EKS + RDS PostgreSQL + S3 + Valkey]
 ROUTE -. failover after recovery .-> AZ[Azure AKS + Azure Database for PostgreSQL + Blob + Managed Redis-compatible cache]
 AWS -->|Encrypted backup + tested restore / CDC as approved| AZ
 PIPE[Portable OCI images + Helm + OpenTofu + OpenTelemetry + OpenAPI] --> AWS
 PIPE --> AZ
 AWS -->|Events via cloud-neutral contract / adapters| BROKER[Broker abstraction]
 AZ --> BROKER
```

## Lead-to-quotation workflow
```mermaid
sequenceDiagram
 participant C as Client
 participant G as Gateway/BFF
 participant R as CRM
 participant P as Party
 participant Q as Quotation
 participant E as Event Bus
 participant W as PDF Worker
 C->>G: POST /v1/leads (idempotency key)
 G->>R: Validated tenant+user context
 R->>R: Tenant RLS, dedupe, write lead+outbox atomically
 R-->>C: 201 Lead ID
 R-->>E: LeadCreated event (outbox publisher)
 C->>G: POST /v1/leads/{id}/convert
 G->>R: Authorize conversion
 R->>P: Resolve/create Party via idempotent API
 P-->>R: Party ID
 R->>R: Create deal / conversion state
 R-->>E: LeadConverted
 C->>G: POST /v1/quotations
 G->>Q: Create quote with Party ID + Deal ID
 Q->>Q: Versioned prices/tax snapshot, tenant sequence, outbox
 Q-->>C: 201 Quote ID
 Q-->>E: QuotationCreated
 E-->>W: Generate PDF
 W-->>Q: PDF reference / status
```

## Implementation notes
- Source baseline prefers modular monolith, NestJS/TypeScript, RabbitMQ and Valkey. Spring Boot, EKS and AWS-managed messaging are proposed alternatives, requiring architecture decision records.
- Prefer a small number of deployable services initially: Core/Entitlements, Party, CRM, Quotation, Integration Hub and asynchronous workers. Separate audit, notification, search and reporting as load/ownership justifies.
- Each service owns tables and migrations; shared RDS cluster with separate schemas/roles initially is acceptable, but no cross-service table access. Tenant RLS and application authorization both mandatory.
- Never accept client-supplied tenant context without validating authenticated membership. Apply entitlement enforcement to REST, consumers, jobs, exports, search, files and cache.
- Use outbox + idempotent inbox + retry/backoff + DLQ; Sagas only for multi-step business transactions. Eventual consistency requires reconciliation and repair tooling.
- Cache-aside for read-heavy, non-sensitive data; tenant/module/permission-scoped keys, short TTLs, explicit invalidation. No cache as system of record.
- Synchronous calls: timeouts, bounded retries with jitter only for safe operations, circuit breaker, bulkheads, trace propagation, API versioning and contract tests.
- Multi-cloud portability is not simultaneous multi-cloud writes: start with AWS production, Azure recovery pilot, encrypted replicated backups and recovery drills; RPO/RTO need confirmation.
- Baseline assumptions from SRS: 100 tenants, 500 concurrent users, 99.5% availability, RPO 15 min, RTO 4 h; proposed targets requiring sign-off.
