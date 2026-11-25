# System Architecture

## 1. Introduction

This document describes the proposed architecture of an event-driven order processing system designed using Python, Django REST Framework, Redis Streams and PostgreSQL.

The architecture separates API handling, event publishing, asynchronous processing and administrative approval to improve maintainability, reliability and scalability.

## 2. High-Level Architecture

The system consists of the following components:

- **Client Application:** Sends order requests and receives processing updates.
- **API Gateway:** Handles incoming HTTP requests, authentication and validation.
- **Order Management:** Manages order creation and lifecycle.
- **Event Publisher:** Publishes order-related events.
- **Redis Streams:** Acts as the event streaming and messaging layer.
- **Background Workers:** Consume and process asynchronous events.
- **Approval Module:** Allows authorized administrators to approve or reject pending orders.
- **PostgreSQL:** Stores orders, processing states and audit information.

## 3. Architecture Flow

### Order Creation

1. Client sends an order creation request.
2. API validates authentication and request data.
3. Order management handles the request.
4. Order is assigned a pending status.
5. An order-created event is published.
6. The API returns an appropriate response to the client.

### Event Processing

1. Background workers consume events from Redis Streams.
2. Workers validate the event and its associated order.
3. Processing status is updated.
4. Failed events are handled using retry and recovery strategies.
5. Successfully processed events are acknowledged.

### Administrative Approval

1. Administrator accesses pending orders.
2. Administrator reviews the order information.
3. Administrator approves or rejects the order.
4. The system records the decision.
5. The order lifecycle is updated accordingly.

## 4. Component Responsibilities

| Component | Responsibility |
|---|---|
| Django REST API | Request handling and validation |
| Order Management | Order lifecycle management |
| Event Publisher | Event creation and publishing |
| Redis Streams | Event distribution |
| Background Workers | Asynchronous processing |
| Approval Module | Administrative decisions |
| PostgreSQL | Persistent data storage |

## 5. Data Flow

The system follows an asynchronous processing approach.

Synchronous operations:
- Authentication
- Request validation
- Initial order creation
- API responses

Asynchronous operations:
- Event consumption
- Background processing
- Retry handling
- Processing status updates

## 6. Reliability and Scalability

The proposed architecture considers the following engineering practices:

- Redis consumer groups for distributed event consumption.
- Idempotency to prevent duplicate processing.
- Retry strategies for temporary failures.
- Dead-letter handling for repeatedly failing events.
- Database transactions for consistent state changes.
- Event acknowledgment after successful processing.
- Horizontal scaling of background workers.
- Transactional outbox pattern to improve reliable event publishing.

## 7. Security Considerations

- Authentication and authorization for API endpoints.
- Role-based administrative access.
- Secure environment variable management.
- Input validation and request sanitization.
- Protection of sensitive customer information.
- Audit logging for administrative actions.
- No credentials or production secrets stored in the repository.

## 8. Technology Choices

**Python and Django REST Framework**

Used for structured backend development and REST API management.

**Redis Streams**

Used as the event streaming mechanism for asynchronous communication between components.

**PostgreSQL**

Used for relational data storage and transaction management.

**Docker**

Proposed for consistent application and worker environments.

## 9. Architectural Design Goals

The main goals of this architecture are:

1. Separation of responsibilities.
2. Reduced coupling between services.
3. Reliable asynchronous processing.
4. Maintainable application structure.
5. Controlled order approval workflow.
6. Scalability under increasing workloads.

## 10. Scope and Implementation Status

This document describes a proposed system architecture and its engineering considerations.

It is a documentation-focused portfolio case study. The architecture should not be interpreted as evidence of a production deployment or tested implementation.
