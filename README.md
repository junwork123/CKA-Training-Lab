# CKA-TrainingLab

## Notice

This Document was created at 03/2023

- Kubernetes version: `1.26`
- `2 hours`
- `15-20` questions
- Pass mark of `66%`
- `Webcam` and `microphone`
- Remotely proctored
- Chrome browser plus an extension
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

- `pods` : po
- `deployments` : deploy
- `services` : svc
- `replicasets` : rs
- `replicationcontollers` : rc
- `configmaps` : cm
- `namespaces` : ns
- `nodes` : no
- `persistentvolumeclaims` : pvc
- `persistentvolumes` : pv
- `resourcequotas` : quota
- `serviceaccounts` : sa
- `daemonsets` : ds
- `cronjobs` : cj
- `ingresses` : ing
- `storageclasses` : sc


### Status Diagnosis

```bash
# Check the status of kubelet
sudo systemctl status kubelet

# Check correct context
k config get-contexts
k config set-context ${CONTEXT NAME} # if you want to change context

# Check the status of System Pod
k logs -n kube-system ${SYSTEM POD NAME}

# Display addresses of the master and services
k cluster-info [dump] # dump option is for more detail

# List all resources in all namespaces, with more details
k get all -A -o wide

# Show related events,  metrics for troubleshooting
k describe pods ${podId}
k top node my-node
k explain deployment --recursive
```

### Create

```bash
# create a deployment
k create deploy nginx --image=nginx 

# create a service & ingress
k create svc clusterip my-service --tcp=80:80 
k create ingress ${Name} --tcp=80:80 

# create a secret & configmap
k create secret generic my-secret --from-file ${FILENAME}
k create configmap my-config --from-literal=special.how=very
```

### Read

if you want to create a resource from the yaml file,

you can use the following command (Don't memorize yaml file)

```bash
# from existing resource
k get pod ${podId} -o yaml > pod.yaml

# from scratch
k create deploy nginx --image=nginx  > deploy.yaml
k create deploy nginx --image=nginx $do > deploy.yaml # # if you have set alias, you can use $do
```

### Update

```bash
# Scale a deployment
k scale deploy my-deployment --replicas=5
```

### Delete

```bash
# Delete a pod
k delete pod ${podId}

# Force delete a pod
k delete pod ${podId} --force --grace-period=0

# Delete all specified resources in current namespace
k delete pod -n test --all 
```

### `RBAC` ( Role Based Access Control )

Only one thing to remember is that the Role and ClusterRole are the same, 

but the Role is in the namespace, and the ClusterRole is not in the namespace.

```bash
k create [role, clusterrole] dev-role \ 
-n default \ # ClusterRole has no namespace
--verb=get,list,watch,update \
--resource=pods \ 
--resource-name=test-pod

k create rolebinding dev-binding \
-n default \ # ClusterRoleBinding has no namespace
--role=dev-role \
--clusterrole=dev-clusterrole \
--serviceaccount=default:test-sa
```

### Specific Situation Command
```bash
# Count the number of pods in a namespace
k get pod -n my-namespace | wc -l

# Check authentication
k auth can-i create deployments -n default --as guest

```

---

### Reference

https://github.com/leandrocostam/cka-preparation-guide

https://github.com/ismet55555/Certified-Kubernetes-Administrator-Notes

https://github.com/arush-sal/cka-practice-environment

https://github.com/kodekloudhub/certified-kubernetes-administrator-course
