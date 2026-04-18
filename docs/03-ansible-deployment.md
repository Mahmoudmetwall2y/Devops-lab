## Objective
Use Ansible from `ansible-host` to:
1. connect to `eks-admin`
2. generate kubeconfig on `eks-admin`
3. copy Kubernetes manifests to `eks-admin`
4. apply a Deployment and a LoadBalancer Service to the EKS cluster

---

## Control and Managed Hosts

### Ansible Control Host
- `ansible-host`

### Managed Host
- `eks-admin`

The Ansible control host connected to `eks-admin` using SSH.

---

## SSH Key Setup
Generated an SSH key pair on `ansible-host`:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/ansible_to_eks_admin -C "ansible-to-eks-admin" -N ""