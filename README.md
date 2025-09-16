## Dynatrace Enhanced k8 Resource Sizer Tool

This tool provides resource sizing recommendations for Dynatrace Kubernetes components, including OneAgent, ActiveGate, and Operator components (Operator, Webhook, CSI driver).
It is aligned with Dynatrace documentation on resource management.

---

### Features

GUI interface for entering cluster and environment details.

Generates YAML with recommended CPU and memory requests/limits.

#### Covers:

-OneAgent
-ActiveGate
-Operator
-Webhook
-CSI driver (init, server, provisioner, registrar, liveness probe).
-YAML is displayed in the GUI and saved to dt_recommendations.yaml.
-Classifies recommendations into groups (small, medium, large).

---

### How to Use

-Download the .exe file.
-Double-click the file to start the GUI.
-Provide the following inputs:
--Cluster Details
---Total number of nodes
---Average vCPU per node
---Average memory per node (GB)
--Environment Details
---Number of namespaces
---Number of pods
---Number of OneAgent versions
---Number of DynaKubes
--Features
---Enable Synthetics
---Enable Log ingest
---Enable Extensions
-Click Generate Recommendations.
-Review the YAML output in the scrollable box.
-A copy of the output is also saved in "dt_recommendations.yaml" in the root folder.

---

### Understanding the Inputs
#### Total number of nodes
The number of worker nodes in your Kubernetes cluster.

#### Average vCPU per node
The average number of vCPUs available per worker node.

#### Average memory per node (GB)
The average memory per node in GB. The tool converts this to GiB automatically.

#### Namespaces
The number of Kubernetes namespaces that Dynatrace is expected to monitor.

#### Pods
The total number of pods in your Kubernetes cluster.

#### OneAgent versions
This refers to the number of distinct OneAgent DaemonSet generations active in the cluster:
-For a new deployment, set this to 1.
-During upgrades or with multiple DynaKubes, this can be 2–4.
-More versions increase the load on Operator and Webhook.

#### DynaKubes
The number of DynaKube custom resources in the cluster. Each may represent a different environment (for example, staging, production).

#### Features
-Enable Synthetics: Add overhead for synthetic monitoring.
-Enable Log ingest: Increases memory requirements.
-Enable Extensions: Increases ActiveGate CPU requirements.

---

## Example Output
dynatrace_recommendations:
  cluster_summary:
    total_nodes: 120
    avg_vcpu_per_node: 8
    avg_memory_per_node_gb: 64.0
    avg_memory_per_node_gib: 59.6
    namespaces: 2500
    pods: 6000
    oneagent_versions: 1
    dynakubes: 2
  oneagent:
    requests:
      cpu: 500m
      memory: 1Gi
    limits:
      cpu: "1"
      memory: 2Gi
    group: medium
  activegate:
    requests:
      cpu: 2500m
      memory: 6Gi
    limits:
      cpu: "4"
      memory: 10Gi
    replicas: 2
    group: medium
  operator_components:
    operator:
      requests:
        cpu: 50m
        memory: 64Mi
      limits:
        cpu: 100m
        memory: 128Mi
    webhook:
      requests:
        cpu: 300m
        memory: 128Mi
      limits:
        cpu: 300m
        memory: 128Mi
    csi_driver:
      csiInit:
        requests:
          cpu: 50m
          memory: 100Mi
        limits:
          cpu: 50m
          memory: 100Mi
      server:
        requests:
          cpu: 50m
          memory: 100Mi
        limits:
          cpu: 50m
          memory: 100Mi
      provisioner:
        requests:
          cpu: 300m
          memory: 100Mi
        limits:
          cpu: 300m
          memory: 100Mi
      registrar:
        requests:
          cpu: 20m
          memory: 30Mi
        limits:
          cpu: 20m
          memory: 30Mi
      livenessProbe:
        requests:
          cpu: 20m
          memory: 30Mi
        limits:
          cpu: 20m
          memory: 30Mi

---

## Notes
-For a new deployment, always set OneAgent versions = 1.
-Groups (small, medium, large) are derived from Dynatrace best practices.
-Use these recommendations as guidance; validate against real workload and Dynatrace support for production sizing.

---

## Author
Developed By: Abhishek Satpathy
Contact: abhishek.satpathy@dynatrace.com
Website: abhisat.com
Version: 1.1
