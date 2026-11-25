# Technical Decisions & Architecture Rationale

## 1. Django REST Framework — API Layer

**Decision:** Django REST Framework (DRF)

**Reasoning:**
- Mature framework for building REST APIs.
- Built-in authentication, permissions, serializers and request validation.
- Structured application architecture suitable for maintainable backend systems.
- Strong ecosystem for integrating background processing and external services.

**Trade-off:**
Django provides many built-in capabilities, but its conventions and ORM abstractions may introduce overhead for extremely lightweight services.

---

## 2. Redis Streams — Event Processing

**Decision:** Redis Streams

**Reasoning:**
- Provides persistent event streams.
- Supports consumer groups for distributed message processing.
- Enables asynchronous processing without blocking API requests.
- Supports message acknowledgment and pending-message recovery.
- Lightweight infrastructure for an event-driven architecture.

**Trade-off:**
Redis Streams is simpler to operate than some enterprise streaming platforms, but it may not be the ideal choice for very large event ecosystems requiring complex stream processing, extensive retention, or multi-team event governance.

---

## 3. PostgreSQL — Primary Database

**Decision:** PostgreSQL

**Reasoning:**
- Reliable relational data storage.
- Strong transactional consistency.
- Supports constraints, indexes and complex queries.
- Suitable for maintaining order records and workflow state.
- Helps preserve data integrity during concurrent operations.

**Trade-off:**
Relational schema design requires careful planning when the data model evolves, and complex high-volume workloads may require additional optimization.

---

## 4. Background Workers — Asynchronous Processing

**Decision:** Dedicated background workers for order processing.

**Reasoning:**
- Keeps API responses fast.
- Separates request handling from business processing.
- Enables independent scaling of worker processes.
- Supports retry mechanisms and failure recovery.
- Prevents long-running operations from blocking API requests.

**Trade-off:**
Asynchronous processing introduces complexity in monitoring, message acknowledgment, retries, duplicate processing and eventual consistency.

---

## 5. Docker — Containerization

**Decision:** Docker

**Reasoning:**
- Consistent development and deployment environments.
- Simplifies service dependency management.
- Supports reproducible deployments.
- Makes local development and infrastructure setup easier.

**Trade-off:**
Container orchestration, networking, storage and debugging require additional operational knowledge.

---

## 6. Event-Driven Architecture

**Decision:** Use events to decouple order submission from downstream processing.

**Reasoning:**
- Reduces direct dependency between API and worker components.
- Improves scalability and service isolation.
- Allows additional consumers to be introduced later.
- Supports asynchronous workflows and independent processing.

**Trade-off:**
Event-driven systems introduce eventual consistency, observability requirements and more complex debugging across components.

---

## 7. Approval Workflow

**Decision:** Separate automated processing from administrative approval.

**Reasoning:**
- Provides a controlled business workflow.
- Ensures processed orders are not finalized without authorization.
- Maintains clear state transitions.
- Supports auditing of approval and rejection decisions.

**Trade-off:**
Approval introduces additional latency and requires careful handling of authorization, audit records and state transition rules.

---

## 8. Reliability and Failure Handling

The proposed architecture considers the following reliability practices:

- Retry mechanisms for transient failures.
- Message acknowledgment only after successful processing.
- Recovery of pending messages.
- Idempotent processing to prevent duplicate effects.
- Structured logging and monitoring.
- Explicit order state management.
- Separation of transient errors from permanent failures.

These practices help prevent message loss, duplicate processing and inconsistent order states.

---

## 9. Future Scalability

The architecture can be extended through:

- Horizontal scaling of background workers.
- Redis Stream consumer groups.
- Database indexing and query optimization.
- Dedicated services for notification or payment processing.
- Rate limiting and API throttling.
- Centralized monitoring and tracing.
- Dead-letter handling for repeatedly failing events.

---

## Summary

The architecture prioritizes maintainability, reliability, separation of concerns and scalability while keeping the infrastructure relatively lightweight.

The technology choices are intended to support an event-driven order workflow without introducing unnecessary operational complexity.

> This repository represents an architectural case study and proposed system design. It is documentation-focused and does not claim a fully implemented or production-deployed system.
