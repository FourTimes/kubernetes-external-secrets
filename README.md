```bash
#Install the external secrets
helm repo add external-secrets https://external-secrets.github.io/kubernetes-external-secrets/
helm repo update
helm install external-secrets external-secrets/kubernetes-external-secrets \
--set env.AWS_REGION=eu-central-1 \
--set securityContext.fsGroup=65534 \
--set serviceAccount.annotations."eks\.amazonaws\.com/role-arn"='arn:aws:iam::xxxxxxx:role/secret-read-role'
```

```bash
# Install the stakater/reloader
helm repo add stakater https://stakater.github.io/stakater-charts
helm repo update
helm install stakater-reloader stakater/reloader

```

IAM Role Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetRandomPassword",
        "secretsmanager:GetResourcePolicy",
        "secretsmanager:GetSecretValue",
        "secretsmanager:DescribeSecret",
        "secretsmanager:ListSecretVersionIds",
        "secretsmanager:ListSecrets"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": ["sts:AssumeRole"],
      "Resource": "*"
    }
  ]
}
```

    Reference => https://acloudguru.com/blog/engineering/fixing-5-common-aws-iam-errors
