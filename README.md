# -

### Network Policy

![network policies](resources/kubernetes-ckad-network-policies-9.jpg)

```yaml
apiVersion: v1
items:
- apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: internal-policy
    namespace: default
  spec:
    podSelector:
      matchLabels:
        name: internal
    egress:
      - to:
        - podSelector:
            matchLabels:
              name: payroll
        ports:
        - port: 8080
          protocol: TCP
      - to:
        - podSelector:
            matchLabels:
              name: mysql
        ports:
        - port: 3306
          protocol: TCP
    policyTypes:
    - Egress
    - Ingress
```
