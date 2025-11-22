---
name: kubernetes-deployer
description: This skill should be used when the user requests Kubernetes deployment configuration, Helm chart creation, K8s manifest generation, container orchestration setup, or cloud-native application deployment. It provides comprehensive Kubernetes deployment patterns and best practices.
---

# Kubernetes Deployer

## Overview

This skill enables comprehensive Kubernetes deployment configuration and Helm chart creation. It generates production-ready K8s manifests, implements best practices for container orchestration, and provides deployment strategies for cloud-native applications.

## When to Use This Skill

Use this skill when the user requests:
- Kubernetes manifest generation
- Helm chart creation
- Deployment configuration
- Service mesh setup
- ConfigMap and Secret management
- Ingress configuration
- Horizontal Pod Autoscaling (HPA)
- StatefulSet for databases
- PersistentVolume configuration
- Kubernetes security best practices

## Core Workflow

### Step 1: Application Deployment

**Basic Deployment**:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: production
  labels:
    app: myapp
    version: v1.0.0
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: myapp
        version: v1.0.0
    spec:
      containers:
      - name: myapp
        image: myregistry/myapp:v1.0.0
        ports:
        - containerPort: 8080
          name: http
          protocol: TCP
        env:
        - name: NODE_ENV
          value: production
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: myapp-secrets
              key: database-url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
          timeoutSeconds: 5
          failureThreshold: 3
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 10
          periodSeconds: 5
          timeoutSeconds: 3
          failureThreshold: 3
        volumeMounts:
        - name: config
          mountPath: /app/config
          readOnly: true
      volumes:
      - name: config
        configMap:
          name: myapp-config
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 1000
      imagePullSecrets:
      - name: registry-credentials
```

### Step 2: Service Configuration

**ClusterIP Service**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp
  namespace: production
  labels:
    app: myapp
spec:
  type: ClusterIP
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
    name: http
  sessionAffinity: ClientIP
```

**LoadBalancer Service**:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-external
  namespace: production
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
  - port: 443
    targetPort: 8080
    protocol: TCP
```

### Step 3: Ingress Configuration

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
  namespace: production
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/rate-limit: "100"
    nginx.ingress.kubernetes.io/proxy-body-size: "10m"
spec:
  tls:
  - hosts:
    - app.example.com
    secretName: myapp-tls
  rules:
  - host: app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: myapp
            port:
              number: 80
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: myapp-api
            port:
              number: 80
```

### Step 4: ConfigMap and Secrets

**ConfigMap**:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myapp-config
  namespace: production
data:
  app.config: |
    server:
      port: 8080
      timeout: 30s
    logging:
      level: info
      format: json
  nginx.conf: |
    server {
      listen 80;
      location / {
        proxy_pass http://backend:8080;
      }
    }
```

**Secret**:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: myapp-secrets
  namespace: production
type: Opaque
stringData:
  database-url: postgresql://user:password@db:5432/myapp
  api-key: super-secret-api-key
  jwt-secret: jwt-signing-secret
```

### Step 5: Horizontal Pod Autoscaler

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: myapp-hpa
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
      - type: Percent
        value: 50
        periodSeconds: 60
    scaleUp:
      stabilizationWindowSeconds: 0
      policies:
      - type: Percent
        value: 100
        periodSeconds: 30
```

### Step 6: StatefulSet for Databases

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
  namespace: production
spec:
  serviceName: postgres
  replicas: 3
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
      - name: postgres
        image: postgres:15
        ports:
        - containerPort: 5432
          name: postgres
        env:
        - name: POSTGRES_PASSWORD
          valueFrom:
            secretKeyRef:
              name: postgres-secret
              key: password
        - name: PGDATA
          value: /var/lib/postgresql/data/pgdata
        volumeMounts:
        - name: postgres-storage
          mountPath: /var/lib/postgresql/data
        resources:
          requests:
            memory: "1Gi"
            cpu: "500m"
          limits:
            memory: "2Gi"
            cpu: "1000m"
  volumeClaimTemplates:
  - metadata:
      name: postgres-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: fast-ssd
      resources:
        requests:
          storage: 20Gi
```

### Step 7: Helm Chart Structure

```
myapp/
├── Chart.yaml
├── values.yaml
├── values-production.yaml
├── values-staging.yaml
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   ├── hpa.yaml
│   ├── serviceaccount.yaml
│   ├── _helpers.tpl
│   └── NOTES.txt
└── charts/
```

**Chart.yaml**:
```yaml
apiVersion: v2
name: myapp
description: My Application Helm Chart
type: application
version: 1.0.0
appVersion: "1.0.0"
keywords:
  - myapp
  - web
maintainers:
  - name: DevOps Team
    email: devops@example.com
```

**values.yaml**:
```yaml
replicaCount: 3

image:
  repository: myregistry/myapp
  tag: "1.0.0"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80
  targetPort: 8080

ingress:
  enabled: true
  className: nginx
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
  hosts:
    - host: app.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: myapp-tls
      hosts:
        - app.example.com

resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70
  targetMemoryUtilizationPercentage: 80

env:
  - name: NODE_ENV
    value: production

secrets:
  database_url: ""
  api_key: ""
```

## Best Practices

**Resource Management**:
- Always define resource requests and limits
- Use QoS classes appropriately
- Monitor resource usage
- Implement autoscaling

**Security**:
- Use RBAC for access control
- Run containers as non-root
- Use Pod Security Policies/Standards
- Scan images for vulnerabilities
- Rotate secrets regularly
- Use network policies

**High Availability**:
- Deploy multiple replicas
- Use Pod Disruption Budgets
- Spread pods across nodes/zones
- Implement health checks
- Use anti-affinity rules

**Configuration**:
- Externalize configuration
- Use ConfigMaps and Secrets
- Version your Helm charts
- Use different values for environments
- Document all configuration options

**Monitoring & Logging**:
- Implement health checks
- Export metrics (Prometheus)
- Centralize logs
- Set up alerting
- Monitor resource usage

## Deployment Strategies

**Rolling Update** (default):
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 0
```

**Blue-Green**:
Deploy new version, switch traffic when ready

**Canary**:
Gradually shift traffic to new version
```yaml
# Use with service mesh or ingress weights
```

**Recreate**:
```yaml
strategy:
  type: Recreate
```
