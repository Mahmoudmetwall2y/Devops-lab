# 05 - Troubleshooting

## 1. EKS Cluster Role Was Not Visible During Cluster Creation

### Problem
The role `EKSClusterServiceRole` did not appear in the EKS cluster creation screen.

### Cause
The role had not been created as the correct EKS cluster service role.

### Fix
Created `EKSClusterServiceRole` with:
- trusted entity type: `AWS service`
- use case: `EKS - Cluster`

Attached:
- `AmazonEKSClusterPolicy`

---

## 2. EKS Auto Mode Warning Appeared During Cluster Creation

### Problem
Warnings appeared requesting additional policies and `sts:TagSession`.

### Cause
EKS Auto Mode was enabled.

### Fix
Disabled EKS Auto Mode and used the standard cluster setup.

This removed the requirement for the extra Auto Mode policies.

---

## 3. `kubectl get nodes` Returned Forbidden

### Problem
After kubeconfig was generated, `kubectl get nodes` returned a `Forbidden` error.

### Cause
The role `DevOpsEKSAccessRole` existed as an EKS access entry, but had no associated access policy.

### Fix
Associated:
- `AmazonEKSClusterAdminPolicy`

with:
- `DevOpsEKSAccessRole`

Scope used:
- `Cluster`

After that, the role had permission to access cluster-scoped resources.

---

## 4. Worker Nodes Were `NotReady`

### Problem
The EKS worker nodes joined the cluster but remained `NotReady`.

### Observed Symptoms
- `aws-node` pods were in `CrashLoopBackOff`
- `coredns` was pending because networking was not healthy

### Cause
The node role did not initially have the required VPC CNI IAM permissions.

### Fix
Attached:
- `AmazonEKS_CNI_Policy`

to:
- `EKSNodegroupRole`

After this fix:
- `aws-node` became `Running`
- `coredns` became `Running`
- nodes became `Ready`

---

## 5. Ansible Localhost AWS Credentials Failure

### Problem
The EventBridge/SNS part of the playbook failed on `localhost`.

### Error