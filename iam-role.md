
#### OpenID connect URL get from AKS

![1](images/_1.png)

![2](images/_2.png)

![3](images/_3.png)

![4](images/_4.png)

![5](images/_5.png)

![6.0](images/_6.0.png)

![6.1](images/_6.1.png)

![7](images/_7.png)

![8](images/_8.png)

![9](images/_9.png)

![10](images/_10.png)

![11](images/_11.png)

![12](images/_12.png)

![13](images/_13.png)

![14](images/_14.png)


IAM role map with service account

```yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app
  namespace: default
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::128506768509:role/secret-read-role

```