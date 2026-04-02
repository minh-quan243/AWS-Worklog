---
title : "Create IAM Policy"
date: "2000-01-01"
weight : 03
chapter : false
pre : " <b> 5.3.3.3. </b> "
---

### Create IAM Quarantine Policy

#### Create IrQuarantineIAMPolicy

1.  **Navigate to IAM Console** → **Policies** → **Create policy**

2.  **Policy JSON**:

<!-- end list -->

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

3.  **Policy name**: `IrQuarantineIAMPolicy`
4.  **Description**: `Deny-all policy for quarantining compromised IAM users`

-----
