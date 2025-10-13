---
name: "Kubernetes Rules"
description: "Rules for AI agents on writing clean, maintainable, and production-ready Kubernetes manifests"
category: "Container Orchestration"
icon: "kubernetes.svg"
version: "1.0"
globs:
  - "**/*.yaml"
  - "**/*.yml"
  - "**/k8s/**"
  - "**/kubernetes/**"
---

- Set memory requests and limits to the same value, only set CPU requests without limits:

```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "256Mi"
    # No CPU limit - allows bursting
```

- Unless asked specifically never use :latest tag, always pin specific versions:

```yaml
# Good
image: nginx:1.25.3

# Bad
image: nginx:latest
```

- Store sensitive data in Secrets, configuration in ConfigMaps:

```yaml
env:
  - name: DATABASE_URL
    valueFrom:
      secretKeyRef:
        name: app-secrets
        key: database-url
  - name: LOG_LEVEL
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: log-level
```

- Use Deployments for stateless apps, StatefulSets for stateful apps
- Use namespaces for isolation and apply resource quotas

- Implement Pod Security Standards and avoid running as root:

```yaml
securityContext:
  runAsNonRoot: true
  runAsUser: 1000
  fsGroup: 1000
  capabilities:
    drop:
      - ALL
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
```

- Use Pod Disruption Budgets to maintain availability during disruptions:

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: app-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: web-app
```

- Always test manifests in non-production environments before applying to production
