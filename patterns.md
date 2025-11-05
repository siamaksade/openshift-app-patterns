## Migration Strategies & Application Architecture Patterns

* **Containerised monolith (lift & shift)**: Packaging existing monolithic app into a single container and deploying on OpenShift as a stateful pod or virtual machine.

* **Strangulating the monolith**: Gradually and piece-by-piece breaking down monolithic applications into smaller, independently deployable services organized around business capabilities, routing specific features to new microservices while the monolith continues to handle remaining functionality.

* **Cloud-native frameworks**: Migrate traditional app servers (e.g. JBoss, Tomcat) to Spring Boot or Quarkus for lower memory footprint, faster startup, and native compilation.

* **Event-driven auto-scaled architecture**: Designing applications around reaction to events, using async messaging to enable loose coupling, scalability, and resilience.

* **CQRS and event sourcing**: Separating read and write operations into different data stores to optimize for performance and scalability.

* **API gateway pattern**: Implementing a single entry point for external traffic that routes requests to appropriate microservices. The gateway handles cross-cutting concerns like authentication, rate limiting, SSL termination, and request transformation before reaching backend services.

## Resilience, Communication & Infrastructure Patterns

* **Circuit breaker and retry logic**: Adding resilience patterns to applications (or through a service mesh) to handle transient failures when calling dependent services through exponential backoff, timeouts, and circuit breakers to prevent cascading failures across the distributed system.

* **Service discovery**: Removing hardcoded service endpoints and instead using Kubernetes Services for DNS-based service discovery. Applications connect to service names rather than IP addresses, letting OpenShift handle load balancing and routing.

* **Sidecar pattern**: Moving cross-cutting concerns like logging, routing and observability into sidecars running in the same pod without modifying application code.

* **Service Mesh**: Deploying a service mesh that automatically instruments all service-to-service communication with metrics, traces, and access logs without modifying application code.

* **Init-container pattern**: Running specialized containers that run to completion before application containers start to perform one-time setup tasks such as cloning data from existing instances, running schema migrations, restoring from backups, or setting up replication configurations.

* **Operator pattern**: Creating Kubernetes operators that encode domain-specific knowledge for managing complex stateful applications, automate tasks like database provisioning, backup scheduling, failover, scaling, and upgrades.

## Configuration & Lifecycle Patterns

* **Externalising configurations**: Moving all configuration data out of application code and container images into Kubernetes ConfigMaps, Secrets and external secret managers. This allows the same container image to run across different environments (dev, staging, production) with environment-specific configuration.

* **Graceful shutdown**: Implementing signal handlers (SIGTERM) and preStop lifecycle hooks to allow applications to complete in-flight requests, close connections, and clean up resources before termination to prevent dropped requests during rolling updates or Pod rescheduling.

* **External cache**: Replacing in-memory session data and state with external data services like Redis, Infinispan, and databases to enable horizontal scaling, rolling updates, and fault tolerance since any Pod instance can handle any request.

* **Health check endpoints**: Creating HTTP endpoints or commands that OpenShift can poll to determine if a application is alive (liveness probe) and ready to accept traffic (readiness probe) to enable proper container lifecycle management. 

* **Umbrella Helm Chart for multi-component releases**: Using umbrella charts to deploy multiple microservices and dependencies together.

## Storage & Observability Patterns

* **Log to standard outputs**: Replace file-based logging with logs written to standard output and error streams to enable OpenShift log aggregator to collect, index, and analyze logs centrally.

* **Observability instrumentation**: Instrumenting applications to expose comprehensive observability signals including metrics in Prometheus format on a /metrics endpoint, and distributed traces that propagate context across service boundaries using standards like OpenTelemetry, enabling visualization of request flows and troubleshoot issues across the distributed system.

* **Auto-scaling on application metrics**: Instrumenting applications with business-relevant metrics (queue depth, request latency, active connections) to enables autoscaling (e.g. Horizontal Pod Autoscaler) based on application-specific indicators.

* **External database-as-a-service**: moving databases entirely outside OpenShift to managed cloud services (AWS RDS, Azure Database, Google Cloud SQL). Applications connect to external endpoints, eliminating the complexity of running databases in OpenShift while leveraging cloud provider expertise in high availability and backups.

* **Database per service**: Assigning each microservice its own dedicated database instance or schema to maintain loose coupling and independent scalability. Each StatefulSet or external database is owned by a single service, preventing cross-service data dependencies.

* **High-performance local volumes**: Using local SSDs or NVMe storage attached directly to nodes for latency- and performance-sensitive stateful workloads with careful pod placement and backup strategies to manage data tied to specific nodes.
