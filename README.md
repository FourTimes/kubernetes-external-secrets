```bash
#Install the external secrets
helm repo add external-secrets https://external-secrets.github.io/kubernetes-external-secrets/
helm repo update
helm install external-secrets external-secrets/kubernetes-external-secrets \
--set env.AWS_REGION=eu-central-1 \
--set securityContext.fsGroup=65534 \
--set serviceAccount.annotations."eks\.amazonaws\.com/role-arn"='arn:aws:iam::xxxxxxx:role/secret-read-role'
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
Create the secret in aws secret manager using awscli

    aws secretsmanager create-secret --name hello-service/password --secret-string "1234"


```yaml
---
apiVersion: "kubernetes-client.io/v1"
kind: ExternalSecret
metadata:
  name: hello-service
spec:
  backendType: secretsManager
  region: eu-central-1
  data:
    - key: hello-service/password
      name: password
```
    Reference => https://acloudguru.com/blog/engineering/fixing-5-common-aws-iam-errors




```bash
# Install the stakater/reloader
helm repo add stakater https://stakater.github.io/stakater-charts
helm repo update
helm install stakater-reloader stakater/reloader

```


```yml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-version-1
  annotations:
    secret.reloader.stakater.com/reload: "hello-service"
spec:
  strategy:
    rollingUpdate:
      maxSurge: 0           #  3 20 30s - 100% 1min down 1000
      maxUnavailable: 1
    type: RollingUpdate
  replicas: 3
  minReadySeconds: 10
  selector:
    matchLabels:
      role: webserver
  template:
    metadata:
      name: web
      labels:
        role: webserver
    spec:
      containers:
      - name: web
        image: jjino/web-app:v1
        ports:
        - name: http
          containerPort: 80
          protocol: TCP
        env:
        - name: BACKEND_USERNAME
          valueFrom:
            secretKeyRef:
              name: hello-service
              key: password
```