## Objective
Create a notification mechanism for any created EC2 instance, including EC2 instances created by the EKS managed node group.

---

## Initial Design
The original plan was to create EventBridge and SNS resources from the Ansible control node (`localhost` play).

### Problem
The Ansible control node did not have AWS credentials configured, so the AWS CLI returned:

- `Unable to locate credentials`

### Fix
Changed the second part of the Ansible playbook to run on:

- `eks-admin`

instead of:

- `localhost`

This worked because `eks-admin` already had the IAM role `DevOpsEKSAccessRole` attached.

---

## Notification Components

### SNS Topic
Created SNS topic:

- `ec2-created-alerts`

### SNS Subscription
Created an email subscription using:
- protocol: `email`
- endpoint: configured email address

### SNS Topic Policy
Configured the SNS topic policy to allow:

- `events.amazonaws.com`

to publish messages to the topic.

---

## EventBridge Rule
Created EventBridge rule:

- `EC2InstanceCreatedNotify`

### Event Pattern Used
Matched:
- source: `aws.ec2`
- detail-type: `EC2 Instance State-change Notification`

Matched states:
- `pending`
- `running`

This configuration captures newly created EC2 instances, including EKS node group instances when they enter instance state changes.

---

## EventBridge Target
Added the SNS topic as the rule target.

Target used:
- SNS topic `ec2-created-alerts`

---

## Result
The EventBridge rule and SNS topic were created successfully through Ansible.

The notification workflow was configured to send an email whenever a matching EC2 instance state-change event occurs.