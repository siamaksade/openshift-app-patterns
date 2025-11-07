## Migration Strategies & Application Architecture Patterns

* **Containerised Monolith (lift & shift)**: Packaging existing monolithic app into a single container and deploying on OpenShift as a stateful pod or virtual machine.

* **Strangulating the Mmonolith**: Gradually and piece-by-piece breaking down monolithic applications into smaller, independently deployable services organized around business capabilities, routing specific features to new microservices while the monolith continues to handle remaining functionality.

* **Cloud-native Frameworks**: Migrate traditional app servers (e.g. JBoss, Tomcat) to Spring Boot or Quarkus for lower memory footprint, faster startup, and native compilation.

* **Event-driven Auto-scaled Architecture**: Designing applications around reaction to events, using async messaging to enable loose coupling, scalability, and resilience.

* **CQRS and Event Sourcing**: Separating read and write operations into different data stores to optimize for performance and scalability.

* **External Cache**: Replacing in-memory session data and state with external data services like Redis, Infinispan, and databases to enable horizontal scaling, rolling updates, and fault tolerance since any Pod instance can handle any request.

## Resilience, Communication & Infrastructure Patterns

* **API Gateway**: Implementing a single entry point for external traffic that routes requests to appropriate microservices. The gateway handles cross-cutting concerns like authentication, rate limiting, SSL termination, and request transformation before reaching backend services.

* **Circuit Breaker and Retry Logic**: Adding resilience patterns to applications (or through a service mesh) to handle transient failures when calling dependent services through exponential backoff, timeouts, and circuit breakers to prevent cascading failures across the distributed system.

* **Service Discovery**: Removing hardcoded service endpoints and instead using Kubernetes Services for DNS-based service discovery. Applications connect to service names rather than IP addresses, letting OpenShift handle load balancing and routing.

* **Sidecars**: Moving cross-cutting concerns like logging, routing and observability into sidecars running in the same pod without modifying application code.

* **Service Mesh**: Deploying a service mesh that automatically instruments all service-to-service communication with metrics, traces, and access logs without modifying application code.

* **Init-container**: Running specialized containers that run to completion before application containers start to perform one-time setup tasks such as cloning data from existing instances, running schema migrations, restoring from backups, or setting up replication configurations.

* **Operator pattern**: Creating Kubernetes operators that encode domain-specific knowledge for managing complex stateful applications, automate tasks like database provisioning, backup scheduling, failover, scaling, and upgrades.

## Configuration Patterns

* **Externalising Configurations**: Moving all configuration data out of application code and container images into Kubernetes ConfigMaps, Secrets and external secret managers. This allows the same container image to run across different environments (dev, staging, production) with environment-specific configuration.

* **Graceful Shutdown**: Implementing signal handlers (SIGTERM) and preStop lifecycle hooks to allow applications to complete in-flight requests, close connections, and clean up resources before termination to prevent dropped requests during rolling updates or Pod rescheduling.

* **Umbrella Chart**
Creating a parent Helm chart that declares multiple sub-charts as dependencies, deploying an entire application stack (frontend, backend, database, cache) as a single unit. The umbrella chart manages inter-chart dependencies and provides unified configuration while maintaining modularity.

* **Library Chart**
Building reusable Helm library charts that contain no deployable resources, only template helpers and common functions. Application charts import these libraries to share template logic, validation functions, and naming conventions across multiple charts, promoting DRY principles.

* **Chart Values Overlay**
 Maintaining multiple values files for different environments (values-dev.yaml, values-staging.yaml, values-prod.yaml) and merging them at deployment time to allow environment-specific overrides while maintaining a common baseline configuration.

* **Kustomize Overlay**
Structuring configurations with a common base directory containing standard Kubernetes manifests, and overlay directories (dev, staging, prod) that apply environment-specific patches. Each overlay references the base and adds transformations using strategic merge patches or JSON patches.

## Storage & Observability Patterns

* **Health check endpoints**: Creating HTTP endpoints or commands that OpenShift can poll to determine if a application is alive (liveness probe) and ready to accept traffic (readiness probe) to enable proper container lifecycle management. 

* **Log to standard outputs**: Replace file-based logging with logs written to standard output and error streams to enable OpenShift log aggregator to collect, index, and analyze logs centrally.

* **Observability Instrumentation**: Instrumenting applications to expose comprehensive observability signals including metrics in Prometheus format on a /metrics endpoint, and distributed traces that propagate context across service boundaries using standards like OpenTelemetry, enabling visualization of request flows and troubleshoot issues across the distributed system.

* **Auto-scaling on Application Metrics**: Instrumenting applications with business-relevant metrics (queue depth, request latency, active connections) to enables autoscaling (e.g. Horizontal Pod Autoscaler) based on application-specific indicators.

* **External Database-as-a-service**: moving databases entirely outside OpenShift to managed cloud services (AWS RDS, Azure Database, Google Cloud SQL). Applications connect to external endpoints, eliminating the complexity of running databases in OpenShift while leveraging cloud provider expertise in high availability and backups.

* **Database per Service**: Assigning each microservice its own dedicated database instance or schema to maintain loose coupling and independent scalability. Each StatefulSet or external database is owned by a single service, preventing cross-service data dependencies.

* **High-performance Local Volumes**: Using local SSDs or NVMe storage attached directly to nodes for latency- and performance-sensitive stateful workloads with careful pod placement and backup strategies to manage data tied to specific nodes.
