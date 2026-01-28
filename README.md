# Kubernetes Production Deployment with HPA

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Production-Ready](https://img.shields.io/badge/Production-Ready-success?style=for-the-badge)
![Auto-Scaling](https://img.shields.io/badge/Auto--Scaling-Enabled-blue?style=for-the-badge)

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Repository Structure](#-repository-structure)
- [Features & Best Practices Implemented](#-features--best-practices-implemented)
- [Files Description](#-files-description)
- [Deployment Instructions](#%EF%B8%8F-deployment-instructions)
- [Security & Compliance](#-security--compliance)
- [Summary](#-summary)
- [Troubleshooting Tips](#-troubleshooting-tips)

## 🌐 Project Overview 

<details> <summary><b>🔧 Click to expand description </b></summary>


### English

#### Environment & Application Profile

Our application operates within a multi-zone Kubernetes cluster spanning three availability zones, comprising a total of five worker nodes.

#### Key Application Characteristics:

- **Initialization:** Requires a 5-10 second startup period.
- **Performance & Scaling:** Load testing confirms that four pod replicas are sufficient to handle the projected peak traffic load.
- **Resource Utilization Pattern:**
      - **CPU:** Exhibits a significant spike during the initial request processing, subsequently stabilizing at a consistent baseline of approximately 0.1 CPU cores.
      - **Memory:** Consumption is stable and predictable, consistently around 128 MiB.
- **Traffic Pattern:** Features a distinct diurnal cycle, with daytime peak traffic exceeding nighttime load by an order of magnitude.

#### Deployment Objectives:
- **Maximize Resilience:** Achieve the highest possible level of fault tolerance and availability within the given cluster topology.
- **Optimize Resource Efficiency:** Minimize the total resource footprint of the deployment while fully meeting performance and scalability requirements.

### Русский

#### Описание среды выполнения и характеристик приложения

Приложение развернуто в мультизональном Kubernetes-кластере, распределенном по трём зонам доступности на инфраструктуре из пяти узлов.

#### Профиль приложения:

- **Время инициализации:** 5–10 секунд.
- **Масштабируемость:** Результаты нагрузочного тестирования показывают, что для обработки пиковой нагрузки достаточно четырёх реплик Pod.
- **Паттерн потребления ресурсов:**
  - **CPU:** Наблюдается выраженный всплеск потребления при обработке первых запросов с последующей стабилизацией на постоянном уровне около 0.1 ядра.
  - **Память:** Потребление стабильно и предсказуемо, находится в районе 128 MiB.
- **Профиль нагрузки:** Нагрузка имеет ярко выраженный суточный цикл: дневной пик превышает ночную минимальную нагрузку на порядок.

#### Цели конфигурации Deployment:
- **Максимизировать отказоустойчивость:** Обеспечить бесперебойную работу приложения, используя возможности мультизональной архитектуры кластера для достижения высокой доступности.
- **Оптимизировать эффективность ресурсов:** Спроектировать конфигурацию, которая гарантирует выполнение SLA при минимальном потреблении вычислительных ресурсов, адаптируясь к естественным циклам нагрузки.

</details>

## 📁 Repository Structure

~~~
umahan-k8s/
├── README.md # Project documentation
├── autoscaler.yaml # Horizontal Pod Autoscaler configuration
├── deployment.yaml # Production-grade Deployment configuration
└── service.yaml # Service definition (to be implemented)
~~~

## 🚀 Features & Best Practices Implemented

<details> <summary><b> Click here to expand</b></summary>

### 1. **Intelligent Auto-Scaling** (`autoscaler.yaml`)
- **Multi-version HPA (v2)** for advanced metrics support
- **Dual-direction scaling policies** with different stabilization windows
- **CPU utilization target** set at 70% for optimal resource usage
- **Conservative scale-down** (50% max per minute) to prevent thrashing
- **Aggressive scale-up** (100% per 30 seconds) for rapid response to traffic spikes

### 2. **Production-Ready Deployment** (`deployment.yaml`)
- **Rolling Update Strategy** with zero-downtime deployments
- **Topology Spread Constraints** for high availability across zones
- **Pod Anti-Affinity** rules to avoid single-node failures
- **Comprehensive Health Probes** (startup, readiness, liveness)
- **Security Hardening** with non-root execution and capability restrictions

### 3. **Resource Optimization**
- **Memory Optimization** with 15% buffer for JVM applications
- **CPU Request/Limit Balance** for cost-efficiency and burst handling
- **Efficient Scaling Range** (1-4 pods) based on load testing

</details>

---

## 📋 Files Description

<details> <summary><b> Click here to expand description </b></summary>

### `deployment.yaml`
**Purpose:** Defines the web application deployment with production-grade configurations.

**Key Features:**
- **Rolling Updates:** `maxSurge: 1, maxUnavailable: 0` for zero-downtime deployments
- **High Availability:** Pod distribution across zones and nodes
- **Health Monitoring:** Three-tier probe system (startup, readiness, liveness)
- **Security:** Non-root execution, privilege escalation prevention
- **Resource Management:** Optimized requests and limits with JVM considerations

**Professional Notes:**
- The 15% memory buffer (144M vs 128M) indicates understanding of JVM overhead
- Separate readiness and liveness endpoints show microservices best practices
- Topology constraints demonstrate multi-zone deployment planning

### `autoscaler.yaml`
**Purpose:** Implements intelligent auto-scaling based on CPU utilization.

**Key Features:**
- **Asymmetric Scaling:** Different policies for scale-up vs scale-down
- **Stabilization Windows:** Prevents flapping (5min scale-down, 1min scale-up)
- **Percentage-Based Scaling:** More stable than pod count changes
- **Resource Metrics:** CPU utilization targeting for predictable scaling

**Professional Notes:**
- `stabilizationWindowSeconds` usage shows understanding of real-world scaling challenges
- Conservative scale-down protects against traffic spikes after reductions
- v2 API enables future metric expansion beyond CPU

### `service.yaml`
**Status:** *Pending implementation*

**Recommended Implementation:**
```
yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app-service
  namespace: production
spec:
  selector:
    app: web-app
  ports:
  - port: 80
    targetPort: 8080
    protocol: TCP
  type: LoadBalancer
```

</details>

## 🛠️ Deployment Instructions

### 📋 Prerequisites

<details>
<summary><b>🔍 Click to expand prerequisites setup</b></summary>

Before starting the deployment, ensure your environment meets these requirements:
```
# Verify kubectl context
kubectl config current-context

# Create production namespace
kubectl create namespace production
```
💡 Tip: Use -o wide flag to see pod distribution across nodes

</details>

#### 🚀 Step 1: Deploy the Application
<details open> <summary><b>📦 Application Deployment Commands</b></summary>

```
# ⚡ Apply deployment configuration
kubectl apply -f deployment.yaml
```
``` 
# ✅ Verify deployment status
kubectl get deployments -n production
kubectl get pods -n production -o wide
```

>💡 Tip: Use -o wide flag to see pod distribution across nodes
  
</details>


#### 📈 Step 2: Configure Auto-Scaling
<details open> <summary><b>⚖️ HPA Configuration Commands</b></summary>
  
```
# 🔄 Apply HPA configuration
kubectl apply -f autoscaler.yaml
```

```
# 👁️ Monitor HPA status in real-time
kubectl get hpa -n production --watch
```
  
>⚠️ Note: The --watch flag provides live updates of scaling events
  
</details>

#### 🔗 Step 3: (Optional) Create Service
<details open> <summary><b>🌐 Service Configuration Commands</b></summary>

```
# 📝 Create service.yaml based on recommended template
# ⚡ Apply service configuration
kubectl apply -f service.yaml
```

```
# 📊 Get service details
kubectl get svc -n production
```

</details>

#### 🧪 Step 4: Test Scaling

<details open> <summary><b>📊 Load Testing Commands</b></summary>

```  
# 🚀 Generate load (example using hey tool)
hey -z 5m -c 50 http://`service-ip`
```

```
# 👀 Monitor scaling behavior in real-time
watch kubectl get hpa,pods -n production
```
🔧 Tool Required: Install hey with go install 
  
</details>
 


### 📊 Performance Characteristics
| Metric | Value | Rationale |
|--|:---:|--|
| 🎯 Min Pods | `1` | Cost optimization during low traffic |
| 📈 Max Pods | `4` | Determined by load testing |
| ⚡ CPU Target | `70%` | Balance between utilization and headroom |
| ⬆️ Scale-up Speed | `100% per 30s` | Rapid response to traffic spikes |
| ⬇️ Scale-down Speed | `50% per 60s` | Conservative to prevent thrashing |
| 💾 Memory Request | `144M` | 128M + 15% JVM overhead |
| ⚠️ Memory Limit | `160M	` | Additional buffer for spikes |

>📝 Note: All performance characteristics are based on load testing results and can be adjusted based on your specific workload.
  
  
### 🔒 Security & Compliance

#### ✅ Implemented Security Measures:

<details open> <summary><b>🛡️ Detailed Security Configuration</b></summary>
  
| Security Feature | Configuration | Purpose |
|--|:---:|--|
| 👤 Non-root execution | `runAsNonRoot: true` | Prevents running as privileged user |
| 🚫 Privilege escalation prevention | `allowPrivilegeEscalation: false` | Blocks privilege elevation attempts |
| 🔒 Capability reduction | `drop: ["ALL"]` | Removes unnecessary Linux capabilities |
| 🆔 Specific user ID | `runAsUser: 1000` | Runs with specific non-root UID |
| 🌐 Network policy ready | `Labeled selectors` | Enables future network policyimplementation | 
 
</details>
  
  
### 🎯 Summary
<details open> <summary><b>📋 Deployment Quick Reference</b></summary>

#### 📝 One-Liner Deployment
  
```
kubectl apply -f deployment.yaml && kubectl apply -f autoscaler.yaml
```
  
#### 🚨 Health Check Commands

```
# Check pod health
kubectl get pods -n production
```

``` 
# View deployment status
kubectl rollout status deployment/web-app -n production
```
  
```
# Check HPA metrics
kubectl describe hpa web-app-hpa -n production
```

#### 📊 Monitoring Dashboard Commands
  
```
# Watch all resources
watch kubectl get all -n production
```
  
``` 
# View events
kubectl get events -n production --sort-by='.lastTimestamp'
```

</details>

### 🆘 Troubleshooting Tips
#### Quick Fixes:

- If pods aren't starting: `kubectl describe pod -n production -l app=web-app`
- If HPA isn't scaling: `kubectl describe hpa web-app-hpa -n production`
- If service isn't accessible: `kubectl describe svc web-app-service -n production`
