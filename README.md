# CKA-TrainingLab

## Notice

This Document was created at 03/2023

- Kubernetes version: 1.26
- 2 hours
- 15-20 questions
- Pass mark of 66%
- Remotely proctored
- Chrome browser plus an extension
- Government-issued non-expired ID
- Webcam and microphone
- Steady internet, preferably 5MB up/down

<br>

---

## Curriculum

Please check detail [CNCF Repo](https://github.com/cncf/curriculum)

### Cluster Architecture, Installation & Configuration (25%)

- Manage role based access control (RBAC).
- Use Kubeadm to install a basic cluster.
- Manage a highly-available Kubernetes cluster.
- Provision underlying infrastructure to deploy a Kubernetes cluster.
- Perform a version upgrade on a Kubernetes cluster using Kubeadm.
- Implement etcd backup and restore.

### Workloads & Scheduling (15%)

- Understand deployments and how to perform rolling update and rollbacks.
- Use ConfigMaps and Secrets to configure applications.
- Know how to scale applications.
- Understand the primitives used to create robust, self-healing, application deployments.
- Understand how resource limits can affect Pod scheduling.
- Awareness of manifest management and common templating tools.

### Services & Networking (20%)

- Understand host networking configuration on the cluster nodes.
- Understand connectivity between Pods.
- Understand ClusterIP, NodePort, LoadBalancer service types and endpoints.
- Know how to use Ingress controllers and Ingress resources.
- Know how to configure and use CoreDNS.
- Choose an appropriate container network interface plugin.

### Storage (10%)

- Understand storage classes, persistent volumes.
- Understand volume mode, access modes and reclaim policies for volumes.
- Understand persistent volume claims primitive.
- Know how to configure applications with persistent storage.

### Troubleshooting (30%)

- Evaluate cluster and node logging.
- Understand how to monitor applications.
- Manage container stdout & stderr logs.
- Troubleshoot application failure.
- Troubleshoot cluster component failure.
- Troubleshoot networking.


<br>

---

## Set Up

For convenience, abbreviate kubectl to k After this section, 

### Auto Completion Setting
```bash
vi ~/.bashrc 

# write down at end of bashrc

# Auto Completion
kubectl completion bash
kubeadm completion bash

# Alias Setting
alias kc="kubectl create" 
export do="--dry-run=client -o yaml" # Create the YAML tamplate (usage: $do)

# Save and Quit

exec $SHELL # Reload shell(Must CAPITAL)
```
```bash
# usage example 
kc deploymentmy my-dep --image=nginx --replicas=3
kc deployment --image=nginx nginx $do
k run --image=busybox $do -- "/bin/sh" "-c" "sleep 36000"
```

<br>

---


## CheatSheet

### Short Words

- pods : po
- deployments : deploy
- services : svc
- replicasets : rs
- replicationcontollers : rc
- configmaps : cm
- namespaces : ns
- nodes : no
- persistentvolumeclaims : pvc
- persistentvolumes : pv
- resourcequotas : quota
- serviceaccounts : sa
- daemonsets : ds
- cronjobs : cj
- ingresses : ing
- storageclasses : sc



### Status Diagnosis

```bash
# Check the status of kubelet
sudo systemctl status kubelet

# Check the status of System Pod
k logs -n kube-system ${SYSTEM POD NAME}

# Display addresses of the master and services
k cluster-info

# Dump current cluster state to stdout
k cluster-info dump

# List the nodes
k get nodes

# List all resources in all namespaces, with more details
k get all -A -o wide

# Show metrics for a given node
k top node my-node

# Use "kubectl describe" for related events and troubleshooting
k describe pods ${podId}

# Use "kubectl explain" to check the structure of a resource object.
k explain deployment --recursive
```

### Create Resource Inline

```bash
# create a pod
k run nginx --image=nginx --restart=Never --dry-run=client -o yaml

# create a deployment
k create deploy nginx --image=nginx --dry-run=client -o yaml

# create a service & ingress
k create svc clusterip my-service --tcp=80:80 --dry-run=client -o yaml
k create ingress ${Name} --tcp=80:80 --dry-run=client -o yaml

# create a secret & configmap
k create secret generic my-secret --from-file ${FILENAME}
k create configmap my-config --from-literal=special.how=very
```

### Edit Resource Inline

```bash
# Change Default Namespace
k config set-context --current --namespace=${toChange}

# Scale a deployment
k scale deploy my-deployment --replicas=5
```

### Delete Resource Inline

```bash
# Delete a pod
k delete pod ${podId}

# Force delete a pod
k delete pod ${podId} --force --grace-period=0

# Delete all specified resources in current namespace
kubectl delete pod -n test --all 
```

### Specific Situation Command
```bash
# Count the number of pods in a namespace
k get pod -n my-namespace | wc -l

```

---

### Reference

https://github.com/leandrocostam/cka-preparation-guide

https://github.com/ismet55555/Certified-Kubernetes-Administrator-Notes

https://github.com/arush-sal/cka-practice-environment

https://github.com/kodekloudhub/certified-kubernetes-administrator-course
