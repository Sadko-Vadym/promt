# Kubernetes Manifest Engineering Portfolio

This repository demonstrates advanced DevOps skills in Kubernetes infrastructure automation through prompt engineering. It contains professionally crafted YAML manifests and the prompts used to generate them, showcasing expertise in containerized application deployment and management.

## 🎯 Project Overview

This portfolio was created to demonstrate proficiency in:
- Kubernetes resource orchestration
- Infrastructure as Code (IaC) best practices
- Prompt engineering for DevOps automation
- Production-ready deployment configurations
- Cloud-native application architecture

## 📋 Manifest Portfolio

Below is a comprehensive table of all Kubernetes manifests with their corresponding prompts, descriptions, and direct links to examples.

| NAME | PROMPT | DESCRIPTION | EXAMPLE |
|------|--------|-------------|---------|
| **app.yaml** | Create a production-ready Kubernetes Deployment manifest for a Go web application named 'go-demo-app' that exposes port 8080. Include: 3 replicas for high availability, proper labels following Kubernetes best practices (app, version, managed-by), a ClusterIP Service to expose the application internally on port 80, and use the container image from ghcr.io/den-vasyliev/go-demo-app:latest. Ensure the manifest includes metadata labels for service discovery and follows the apps/v1 API version. | Basic production deployment with service discovery, load balancing across 3 replicas, and internal service exposure | [app.yaml](yaml/app.yaml) |
| **app-livenessProbe.yaml** | Generate a Kubernetes Deployment manifest for 'go-demo-app' with a comprehensive liveness probe configuration. The deployment should include: an HTTP-based liveness probe checking the /healthz endpoint on port 8080, initial delay of 30 seconds to allow application startup, probe interval of 10 seconds, timeout of 5 seconds for each probe attempt, and failure threshold of 3 consecutive failures before container restart. Include proper resource labels, 3 replicas, and a Service manifest. This ensures automatic recovery from application deadlock or hung states. | Deployment with health monitoring that automatically restarts containers when the application becomes unresponsive or enters a deadlock state | [app-livenessProbe.yaml](yaml/app-livenessProbe.yaml) |
| **app-readinessProbe.yaml** | Create a Kubernetes Deployment manifest for 'go-demo-app' with both liveness and readiness probes for comprehensive health checking. Include: liveness probe on /healthz with 30s initial delay and 10s interval to detect hung processes, readiness probe on /readyz with 5s initial delay and 5s interval to control traffic routing, both probes using HTTP GET method on port 8080, proper timeout and failure threshold settings. The readiness probe should prevent traffic routing to pods that aren't ready to serve requests, while liveness ensures failed pods are restarted. Include Service configuration. | Deployment with dual health check system: liveness probe for automatic recovery and readiness probe for traffic management, ensuring zero-downtime deployments | [app-readinessProbe.yaml](yaml/app-readinessProbe.yaml) |
| **app-volumeMounts.yaml** | Generate a comprehensive Kubernetes Deployment for 'go-demo-app' demonstrating three types of volume mounts: 1) ConfigMap volume for read-only application configuration mounted at /app/config containing app.conf file, 2) PersistentVolumeClaim (5Gi, standard storage class) for persistent data storage at /app/data with ReadWriteOnce access, 3) emptyDir volume for temporary cache at /app/cache with 1Gi size limit. Include the ConfigMap definition with application configuration, PVC manifest requesting 5Gi storage, proper volume mount configurations with readOnly flags where appropriate, and a Service. Demonstrate production-ready volume management for configuration, persistence, and ephemeral storage. | Deployment showcasing three volume patterns: immutable configuration via ConfigMap, persistent data storage via PVC, and temporary cache with emptyDir | [app-volumeMounts.yaml](yaml/app-volumeMounts.yaml) |
| **app-cronjob.yaml** | Create a production-grade Kubernetes CronJob manifest for automated backup operations. Requirements: schedule to run daily at 2 AM UTC using cron expression "0 2 * * *", concurrencyPolicy set to Forbid to prevent overlapping executions, job history limits (3 successful, 1 failed), 300s starting deadline, backoff limit of 3 retries, active deadline of 600 seconds. The job container should execute a shell script that creates timestamped tar.gz backups of application data, implements automatic cleanup of backups older than 7 days, includes proper logging with timestamps, uses environment variables for configuration (BACKUP_RETENTION_DAYS, BACKUP_PATH). Include volumeMounts for backup storage PVC (20Gi) and read-only access to application data PVC. Add resource requests and limits. | Automated daily backup CronJob with intelligent scheduling, resource management, automatic cleanup, and failure handling for production data protection | [app-cronjob.yaml](yaml/app-cronjob.yaml) |
| **app-job.yaml** | Generate a Kubernetes Job manifest for one-time database migration execution with production safety features. Include: backoffLimit of 3 for retry logic, activeDeadlineSeconds of 600 to prevent hanging jobs, ttlSecondsAfterFinished set to 86400 (24h) for automatic cleanup, completions and parallelism both set to 1 for sequential execution, restartPolicy OnFailure. The job should run a migration container executing database schema updates with proper logging, environment variables sourced from Secrets (DATABASE_URL) and direct values (MIGRATION_VERSION, ENVIRONMENT), resource requests and limits, and an initContainer using busybox to wait for database availability before migration starts. Include Secret manifest with database connection string. | One-time database migration job with pre-flight checks, automatic retry logic, timeout protection, and automatic cleanup after completion | [app-job.yaml](yaml/app-job.yaml) |
| **app-multicontainer.yaml** | Create an advanced Kubernetes Deployment manifest demonstrating a multi-container pod pattern with three containers: 1) Main application container (go-demo-app) with proper resource limits, 2) Log aggregator sidecar using fluent-bit:2.2 for log collection and forwarding to Elasticsearch, 3) Metrics exporter sidecar using statsd-exporter for Prometheus metrics collection. Include: initContainer for configuration initialization using busybox, shared volumes (emptyDir) for inter-container communication of config and logs, ConfigMap for fluent-bit configuration with log parsing rules, proper port definitions for HTTP (8080), metrics (9090), and statsd (9125 UDP), pod annotations for Prometheus scraping, resource requests and limits for all containers. Include Service exposing both HTTP and metrics ports. | Advanced multi-container pod architecture with main application, log aggregation sidecar, metrics collection sidecar, and shared volume communication | [app-multicontainer.yaml](yaml/app-multicontainer.yaml) |
| **app-resources.yaml** | Generate a production-ready Kubernetes Deployment with comprehensive resource management and horizontal autoscaling. Include: Deployment with resource requests (250m CPU, 256Mi memory, 1Gi ephemeral storage) and limits (1000m CPU, 512Mi memory, 2Gi ephemeral storage), both liveness and readiness probes for health monitoring, environment variables sourced from resource field refs (GOMAXPROCS from CPU limits, MEMORY_LIMIT from memory limits) to enable application self-tuning, pod anti-affinity rules to spread pods across nodes for high availability, RollingUpdate strategy with maxSurge 1 and maxUnavailable 0 for zero-downtime deployments. Add HorizontalPodAutoscaler (HPA) manifest configured to scale from 3 to 10 replicas based on 70% CPU and 80% memory utilization, with intelligent scaling behavior (gradual scale-down, aggressive scale-up) and stabilization windows. Include Service manifest. | Complete resource optimization setup with auto-scaling, resource quotas, efficient pod distribution, and intelligent scaling policies for production workloads | [app-resources.yaml](yaml/app-resources.yaml) |
| **app-secret-env.yaml** | Create a Kubernetes Deployment for 'go-demo-app' demonstrating secure secrets management and environment variable injection. Include: multiple environment variables sourced from Secret (sensitive data: database credentials including host/port/name/user/password, API keys, JWT secret, encryption key, Redis password) and ConfigMap (non-sensitive configuration: environment name, log level, Redis host, feature flags JSON), proper Secret manifest with type Opaque and stringData for all sensitive values, ConfigMap with application configuration including JSON feature flags, proper label organization. The deployment should show best practices for separating sensitive vs non-sensitive configuration, use valueFrom with secretKeyRef and configMapKeyRef for environment variable injection, and include resource limits. Add Service manifest. | Deployment demonstrating secure environment variable management with separation of secrets and config, showcasing proper credential handling for databases, APIs, and application settings | [app-secret-env.yaml](yaml/app-secret-env.yaml) |

## 🚀 Key Features Demonstrated

### High Availability & Resilience
- Multi-replica deployments (3+ replicas)
- Pod anti-affinity for distribution across nodes
- Health checks (liveness and readiness probes)
- Automatic recovery from failures

### Scalability & Performance
- Horizontal Pod Autoscaling (HPA) based on CPU/memory
- Resource requests and limits for efficient scheduling
- Intelligent scaling policies (gradual scale-down, fast scale-up)
- Performance tuning via resource field refs

### Security & Configuration Management
- Secrets management for sensitive data
- ConfigMaps for application configuration
- Read-only volume mounts for immutable config
- Separation of concerns (secrets vs config)

### Storage & Persistence
- PersistentVolumeClaims for stateful data
- ConfigMap volumes for configuration files
- emptyDir for temporary storage
- Multiple volume types in single pod

### Observability & Operations
- Multi-container patterns (sidecars for logging and metrics)
- Prometheus metrics integration
- Structured logging with log aggregation
- Health check endpoints

### Automation & Maintenance
- CronJobs for scheduled backup operations
- Jobs for one-time migrations
- Automatic cleanup policies (TTL)
- Init containers for pre-flight checks

## 📚 Prompt Engineering Methodology

This portfolio follows Google's Prompt Engineering best practices:

### 1. **Specificity & Context**
Each prompt provides detailed requirements, exact parameter values, and context about the intended use case, ensuring consistent and production-ready outputs.

### 2. **Structured Instructions**
Prompts use numbered lists, clear sections, and explicit requirements to guide the generation of complex, multi-resource manifests.

### 3. **Examples & Constraints**
Prompts include specific values (e.g., "3 replicas", "port 8080", "30s initial delay") rather than generic placeholders, demonstrating real-world configuration.

### 4. **Progressive Complexity**
Manifests progress from simple deployments to complex multi-container patterns, showcasing incremental skill development.

### 5. **Best Practices Integration**
Every prompt explicitly requests adherence to Kubernetes best practices, proper labeling, and production-ready configurations.

## 🛠️ Technical Stack

- **Container Orchestration**: Kubernetes (apps/v1, batch/v1, autoscaling/v2)
- **Application**: Go-based web application
- **Container Registry**: GitHub Container Registry (ghcr.io)
- **Monitoring**: Prometheus, Fluent Bit, StatsD
- **Storage**: PersistentVolumes, ConfigMaps, Secrets
- **Automation**: CronJobs, Jobs, Init Containers

## 📖 Usage

### Viewing Manifests
All manifests are located in the `yaml/` directory and can be reviewed individually:

```bash
# View basic deployment
cat yaml/app.yaml

# View deployment with health checks
cat yaml/app-livenessProbe.yaml

# View multi-container pattern
cat yaml/app-multicontainer.yaml
```

### Applying to Kubernetes Cluster
While this portfolio focuses on manifest engineering rather than deployment, the manifests can be applied to a Kubernetes cluster:

```bash
# Apply basic deployment
kubectl apply -f yaml/app.yaml

# Apply deployment with autoscaling
kubectl apply -f yaml/app-resources.yaml

# Apply backup CronJob
kubectl apply -f yaml/app-cronjob.yaml
```

### Validation
Validate manifests without applying them:

```bash
# Validate syntax
kubectl apply -f yaml/app.yaml --dry-run=client

# Validate with server-side checks
kubectl apply -f yaml/app.yaml --dry-run=server
```

## 🎓 Learning Resources

This portfolio was developed using the following resources:

- [Google Cloud Prompt Engineering Guide](https://www.kaggle.com/whitepaper-prompt-engineering)
- [kubectl-ai by Google Cloud Platform](https://github.com/GoogleCloudPlatform/kubectl-ai)
- [Kubernetes Official Documentation](https://kubernetes.io/docs/)
- [Kubernetes Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)

## 💼 Professional Context

This portfolio demonstrates:
- **DevOps Engineering**: Infrastructure automation and container orchestration
- **Cloud-Native Architecture**: Microservices deployment patterns
- **Prompt Engineering**: AI-assisted infrastructure development
- **Production Operations**: Real-world deployment strategies
- **Security Best Practices**: Secrets management and secure configuration

## 📧 Contact

This portfolio was created as part of a contractor application for a marketing automation project requiring DevOps and prompt engineering expertise.

---

**Repository Structure:**
```
.
├── README.md                          # This file
├── yaml/                              # Kubernetes manifests directory
│   ├── app.yaml                       # Basic deployment
│   ├── app-livenessProbe.yaml        # Health monitoring
│   ├── app-readinessProbe.yaml       # Traffic management
│   ├── app-volumeMounts.yaml         # Storage patterns
│   ├── app-cronjob.yaml              # Scheduled tasks
│   ├── app-job.yaml                  # One-time jobs
│   ├── app-multicontainer.yaml       # Multi-container pods
│   ├── app-resources.yaml            # Resource management & autoscaling
│   └── app-secret-env.yaml           # Secrets management
└── .gitignore                        # Git ignore rules
```

**Last Updated:** December 24, 2025

