cat > docs/01-aws-setup.md <<'EOF'
# 01 - AWS Setup

## Objective
Implement the AWS part of the task:
1. Create IAM group `devops`
2. Create IAM users `user1` and `user2`
3. Create IAM role and policy to assume the role
4. Attach the assume-role policy to group `devops`
5. Create EKS cluster and add the role in EKS Access Entries
6. Create EC2 instance to access the cluster

---

## IAM Group
Created IAM group:

- `devops`

This group was used to assign the same assume-role permission to both IAM users.

---

## IAM Users
Created IAM users:

- `user1`
- `user2`

Both users were added to the IAM group:

- `devops`

---

## EKS Access Role
Created IAM role:

- `DevOpsEKSAccessRole`

This role was used for:

- EKS Access Entry
- EC2 admin instance access to the cluster

### Trust Policy Used for `DevOpsEKSAccessRole`

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSameAccountToAssume",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::586794484505:root"
      },
      "Action": "sts:AssumeRole"
    },
    {
      "Sid": "AllowEC2ToAssume",
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}