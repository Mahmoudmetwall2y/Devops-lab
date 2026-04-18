# EKS + Ansible + EventBridge Lab

## Project Objective

This project documents the full implementation of two tasks:

### Task 1: AWS
1. Create IAM group named `devops`
2. Create two IAM users: `user1` and `user2`
3. Create IAM role and policy to allow assuming this role
4. Attach the assume-role policy to group `devops`
5. Create an Amazon EKS cluster and add the role in EKS Access Entries
6. Create an EC2 instance to access the cluster

### Task 2: Using Ansible
1. Access the EC2 instance and create kubeconfig using the role added in EKS Access Entries
2. Use Ansible `copy` module to copy Kubernetes manifests to the EC2 instance, then create a Deployment and a LoadBalancer Service
3. Create EventBridge notification for any created EC2 instance, including EKS node group instances
4. Document all AWS and Ansible steps
5. Push all code and documentation to GitHub

---

## Final Architecture

- **IAM group**: `devops`
- **IAM users**: `user1`, `user2`
- **EKS access role**: `DevOpsEKSAccessRole`
- **EKS cluster role**: `EKSClusterServiceRole`
- **EKS node role**: `EKSNodegroupRole`
- **EKS cluster**: `devops-eks`
- **Admin EC2 instance**: `eks-admin`
- **Ansible control EC2 instance**: `ansible-host`
- **SNS topic**: `ec2-created-alerts`
- **EventBridge rule**: `EC2InstanceCreatedNotify`

---

## Repository Files

- `inventory.ini` → Ansible inventory
- `ansible.cfg` → Ansible configuration
- `playbook.yml` → Full Ansible automation
- `files/deployment.yaml` → Kubernetes Deployment manifest
- `files/service.yaml` → Kubernetes Service manifest
- `docs/01-aws-setup.md` → IAM and EKS setup steps
- `docs/02-eks-access-and-ec2.md` → EKS access and EC2 access steps
- `docs/03-ansible-deployment.md` → Ansible deployment steps
- `docs/04-eventbridge-sns.md` → EventBridge and SNS notification steps
- `docs/05-troubleshooting.md` → Problems encountered and fixes applied

---

## Verification Summary

At the end of the implementation:

- `kubectl get nodes` returned both nodes in `Ready` state
- `kubectl get pods -n kube-system -o wide` returned:
  - `aws-node` running
  - `coredns` running
  - `kube-proxy` running
  - `metrics-server` running
- Ansible copied Kubernetes manifests and applied them successfully
- EventBridge and SNS notification setup was automated with Ansible
- EKS cluster was accessible from the `eks-admin` EC2 instance through the attached IAM role and EKS Access Entry

---

## GitHub Push

```bash
git init
git add .
git commit -m "Document AWS and Ansible EKS lab"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main