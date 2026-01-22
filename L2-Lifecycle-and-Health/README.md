# 🔥 PROJECT PHOENIX – MISSION L2: THE GATEKEEPERS
## **Pod Lifecycle & Health Probes**

---

## 📡 MISSION BRIEFING

**Commander**, the Phoenix system is locked behind a multi-stage security protocol. Your deployment is **stuck in limbo** — not dead, not alive. The gatekeepers (Init Containers, Startup Probes, Liveness & Readiness Probes) are **misconfigured**, and the system refuses to launch.

**Intelligence reports 5 critical errors** blocking the Phoenix server from reaching operational status. Your mission: **identify and neutralize each failure point** to bring the system fully online.

⏰ **Time is critical.** Every second the system stays down, the mission falls further behind.

---

## 🎯 MISSION OBJECTIVE

Deploy the **Phoenix Gatekeeper** application to the `phoenix-system-2` namespace with:
- ✅ All Init Containers passing
- ✅ All Health Probes configured correctly
- ✅ Pod status: **1/1 READY**
- ✅ Service responding at `http://<EXTERNAL-IP>/`

**Victory message:** `MISSION ACCOMPLISHED: PHOENIX GATEKEEPER IS ONLINE`

---

## 📂 MISSION FILES

| File | Description | Difficulty |
|------|-------------|------------|
| **01-phoenix-challenge.yaml** | 🌑 **Dark Mode** – No hints, no mercy. Debug using only `kubectl` and your skills. | ⚡⚡⚡ |
| **02-phoenix-guided.yaml** | 🔦 **Intel Mode** – Error markers explain the "Why" behind each failure. | ⚡⚡ |
| **03-phoenix-solution.yaml** | ✅ **Reference Solution** – The fully operational configuration. | Reference Only |

---

## 🛠️ TROUBLESHOOTING INTEL

### **Core Commands for This Mission**

| Command | Purpose |
|---------|---------|
| `kubectl -n phoenix-system-2 get pods` | Check pod status and current phase |
| `kubectl -n phoenix-system-2 describe pod <pod-name>` | Deep dive into events, probe failures, and container states |
| `kubectl -n phoenix-system-2 logs <pod-name>` | View application logs (if container starts) |

### **Advanced Recon (Optional)**

```bash
# Watch pod status in real-time
kubectl -n phoenix-system-2 get pods -w

# Check events across the namespace
kubectl -n phoenix-system-2 get events --sort-by='.lastTimestamp'

# Verify service endpoint
kubectl -n phoenix-system-2 get svc phoenix-service
```

---

## 🚨 ERRORS COVERED IN THIS MISSION

This lab intentionally includes **5 critical misconfigurations**:

### **1️⃣ Init Container Dependency Deadlock**
- **Symptom:** Pod stuck at `Init:0/1` indefinitely
- **Root Cause:** Waiting for a non-existent service (`db-backend-prod`)
- **Learning Goal:** Understand Init Container blocking behavior

### **2️⃣ Startup Probe OCI Runtime Failure**
- **Symptom:** Pod status shows `CrashLoopBackOff` or probe failure
- **Root Cause:** Startup probe executes a non-existent script (`verify-payload.sh`)
- **Learning Goal:** Difference between `exec`, `httpGet`, and `tcpSocket` probes

### **3️⃣ Liveness Probe Premature Restart**
- **Symptom:** Pod keeps restarting during initialization
- **Root Cause:** Liveness probe starts too early (1 second delay) before the app is ready
- **Learning Goal:** Proper `initialDelaySeconds` configuration for slow-starting apps

### **4️⃣ Readiness Probe Port Mismatch**
- **Symptom:** Pod shows `0/1 READY` despite running
- **Root Cause:** Readiness probe checks port `8080` instead of `8000`
- **Learning Goal:** Impact of readiness on service routing

### **5️⃣ Readiness Probe Excessive Success Threshold**
- **Symptom:** Pod takes **50 seconds** to become Ready
- **Root Cause:** `successThreshold: 10` requires 10 consecutive passes
- **Learning Goal:** Balancing probe sensitivity vs. availability

---

## ✅ SUCCESS CRITERIA

Your mission is **COMPLETE** when you achieve:

| Checkpoint | Validation Command | Expected Output |
|------------|-------------------|-----------------|
| **Pod Running** | `kubectl -n phoenix-system-2 get pods` | `phoenix-deployment-xxxxx   1/1   Running` |
| **All Probes Passing** | `kubectl -n phoenix-system-2 describe pod <pod>` | No probe failure events |
| **Service Accessible** | `curl http://<EXTERNAL-IP>/` | `{"message":"MISSION ACCOMPLISHED: PHOENIX GATEKEEPER IS ONLINE"}` |
| **Health Endpoint Working** | `curl http://<EXTERNAL-IP>/health` | `{"status":"ok"}` |

---

## 🎓 LEARNING OUTCOMES

By completing this mission, you will master:

- ✅ **Init Container patterns** and dependency management
- ✅ **Startup Probe configuration** for slow-starting applications
- ✅ **Liveness Probe tuning** to prevent restart loops
- ✅ **Readiness Probe debugging** and service traffic routing
- ✅ **Pod lifecycle phases** (`Pending`, `Init`, `Running`, `Ready`)
- ✅ **Event-driven troubleshooting** using `kubectl describe`

---

## 🚀 DEPLOYMENT INSTRUCTIONS

### **Mode 1: Challenge (Recommended for Learning)**
```bash
# Deploy the broken configuration
kubectl apply -f 01-phoenix-challenge.yaml

# Begin troubleshooting
kubectl -n phoenix-system-2 get pods
kubectl -n phoenix-system-2 describe pod <pod-name>

# Fix errors iteratively and redeploy
kubectl apply -f 01-phoenix-challenge.yaml
```

### **Mode 2: Guided (With Hints)**
```bash
# Deploy with error explanations
kubectl apply -f 02-phoenix-guided.yaml

# Errors are documented inline - read the comments!
```

### **Mode 3: Solution (Reference)**
```bash
# Deploy the working configuration
kubectl apply -f 03-phoenix-solution.yaml

# Verify success
kubectl -n phoenix-system-2 get pods
```

---

## 🧹 CLEANUP

```bash
# Remove all resources when done
kubectl delete namespace phoenix-system-2
```

---

## 💡 COMMANDER'S TIPS

1. **Start with `kubectl describe pod`** – It shows the exact probe failure reasons
2. **Watch the Events section** – It's a timeline of what went wrong
3. **Liveness kills, Readiness isolates** – Know when to use which
4. **Init Containers block everything** – They must succeed before the main container starts
5. **Port mismatches are silent killers** – Always verify `containerPort` vs probe `port`

---

## 📚 REFERENCE DOCUMENTATION

- [Kubernetes Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)
- [Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
- [Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

---

**🔥 Good luck, Commander. The Phoenix awaits your command.**
