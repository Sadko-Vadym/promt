# Kubernetes Manifest Engineering Portfolio

This repository demonstrates DevOps skills in Kubernetes infrastructure automation through prompt engineering.

## 📋 Manifest Portfolio

| NAME | PROMPT | DESCRIPTION | EXAMPLE |
|------|--------|-------------|---------|
| **app.yaml** | Create a Kubernetes Pod manifest for a Go application named 'app' with labels app=demo and run=demo, using image ghcr.io/den-vasyliev/go-demo-app:latest, exposing container port 8080 named http. | Basic Pod deployment with container port exposure | [app.yaml](yaml/app.yaml) |
| **app-livenessProbe.yaml** | Generate a Kubernetes Pod manifest with a liveness probe. Use image ghcr.io/den-vasyliev/go-demo-app:latest, add an HTTP GET liveness probe on path /healthz port 8080 with initialDelaySeconds 5, timeoutSeconds 1, periodSeconds 10, failureThreshold 3. | Pod with liveness probe for automatic container restart on failure | [app-livenessProbe.yaml](yaml/app-livenessProbe.yaml) |
| **app-readinessProbe.yaml** | Create a Pod manifest with both liveness and readiness probes. Include liveness probe on /healthz and readiness probe on /ready path, both on port 8080. Set readiness periodSeconds to 2, initialDelaySeconds to 0, failureThreshold 3, successThreshold 1. | Pod with liveness and readiness probes for health monitoring | [app-readinessProbe.yaml](yaml/app-readinessProbe.yaml) |
| **app-volumeMounts.yaml** | Generate a Pod manifest with volumeMounts. Include liveness and readiness probes, mount a hostPath volume at /data using path /var/lib/app. | Pod with volume mounts demonstrating persistent storage | [app-volumeMounts.yaml](yaml/app-volumeMounts.yaml) |
| **app-cronjob.yaml** | Create a Kubernetes CronJob manifest named app-cronjob with schedule "*/5 * * * *". Use bash image and command echo Hello world. Set restartPolicy to OnFailure. | CronJob for scheduled task execution | [app-cronjob.yaml](yaml/app-cronjob.yaml) |
| **app-job.yaml** | Generate a Kubernetes Job manifest with initContainers. Include an init container using busybox for initialization, main container using go-demo-app image, two emptyDir volumes for data-input and data-output, backoffLimit 0, restartPolicy Never. | Job with init container for one-time task execution | [app-job.yaml](yaml/app-job.yaml) |
| **app-multicontainer.yaml** | Create a multi-container Pod manifest named app-two-containers. Include nginx container and debian container sharing an emptyDir volume named html. The debian container should run a loop writing date to /html/index.html. | Multi-container Pod with shared volume | [app-multicontainer.yaml](yaml/app-multicontainer.yaml) |
| **app-resources.yaml** | Generate a Pod manifest with resource requests and limits. Set requests to 100m CPU and 128Mi memory, limits to 1000m CPU and 256Mi memory. Include liveness and readiness probes. | Pod with resource constraints for scheduling | [app-resources.yaml](yaml/app-resources.yaml) |
| **app-secret-env.yaml** | Create a Pod manifest that uses secrets as environment variables. Include env vars SECRET_USERNAME and SECRET_PASSWORD sourced from a secret named mysecret with keys username and password. Set restartPolicy to Never. | Pod with secret environment variables | [app-secret-env.yaml](yaml/app-secret-env.yaml) |
