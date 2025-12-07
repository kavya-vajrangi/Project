# Project
Comparative Analysis of Kubernetes Schedulers for Bigdata

This repository contains my M.Tech dissertation project comparing the default Kubernetes scheduler with the Volcano batch scheduler for big data workloads such as Apache Spark.  
The study was carried out at Wipro Technologies and submitted to BITS Pilani in November 2025.

📖 Abstract

Big data applications increasingly rely on distributed frameworks like Apache Spark, often deployed on Kubernetes.  
While Kubernetes excels at container orchestration, its default scheduler struggles with batch workloads due to **resource deadlocks** and lack of **gang scheduling**.  

This project presents an empirical performance comparison between:
- Default Kubernetes Scheduler → prone to deadlocks, inconsistent job completion.
- Volcano Scheduler → job‑aware, gang scheduling enabled, 100% job success rate.

Key findings:
- Volcano completed jobs in ~2.5 minutes vs 9.5 minutes with the default scheduler.
- Scheduler throughput improved by ~100x (~0.98 pods/s vs. ~0.01 pods/s).
- Latency reduced (p99: 112 ms vs. 158 ms).
- Minimal overhead in CPU (~0.02 cores) and memory (~4 MiB).

⚙️ Methodology

1. Environment Setup
   - Multi‑node Kubernetes cluster (1 control plane, 3–7 worker nodes).
   - Prometheus + Grafana monitoring stack.

2. Workload
   - Apache Spark jobs submitted via Spark Operator.
   - Benchmarks defined in YAML manifests.

3. Schedulers Tested
   - Default kube‑scheduler (baseline).
   - Volcano scheduler (`schedulerName: volcano`).

4. Metrics Collected
   - Job success rate
   - Job completion time
   - Scheduler throughput
   - Scheduler latency (p99)
   - CPU & memory usage

📊 Results Summary
Job Stability: The default scheduler is unreliable for concurrent Spark jobs, frequently causing deadlocks. Volcano is 100% reliable due to its "all-or-nothing" gang scheduling.
Performance & Throughput: Volcano is not just more stable, it is significantly faster. By eliminating idle wait times and managing queues efficiently, it completed jobs faster and handled a workload more efficiently in our tests.

The "Why": Application-Awareness:
The default scheduler sees individual pods.
Volcano sees the entire job as one cohesive unit.
This fundamental difference is the reason for its superior performance

For big data workloads on Kubernetes, the choice of scheduler is critical.
The Default Kubernetes Scheduler is unsuitable for concurrent Spark jobs due to its application-unaware design, which leads to instability and inefficiency.
The Volcano Scheduler provides superior performance, achieving 100% job success, significantly faster completion times, and higher cluster throughput.

Final Takeaway: Adopting an application-aware scheduler like Volcano is not just an optimization, it is a foundational requirement for building a stable and efficient data processing platform on Kubernetes.






