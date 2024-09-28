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

### Persistent Volume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-log
spec:
  accessModes:
    - ReadWriteMany
  capacity:
    storage: 100Mi
  hostPath:
    path: /pv/log
```

### Persistent Volume Claim

![PV-PVC-POD](resources/pv-pvc.png)


```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: claim-log-1
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 50Mi
```

### Mount Volume with Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webapp
spec:
  containers:
  - image: kodekloud/event-simulator
    imagePullPolicy: Always
    name: event-simulator
    volumeMounts:
    - mountPath: /log
      name: app-log
  volumes:
  - persistentVolumeClaim:
      claimName: claim-log-1
    name: app-log
```



OUT OF SYLLABUS

Static Provisioning
![Static Provisioning](resources/static-provisioning.png)


Dynamic Provisioning
![Dynamic Provisioning](resources/dynamic-provisioning.png)

# Security

##### kubeconfig file
location .kube directory
```yaml
apiVersion: v1
current-context: orbstack
kind: Config
preferences: {}
clusters:
  - cluster:
      certificate-authority-data: dummydata
      server: https://0.0.0.0:49630
    name: k3d-mycluster
  - cluster:
      certificate-authority-data: dummydata
      server: https://127.0.0.1:26444
    name: orbstack
contexts:
  - context:
      cluster: k3d-mycluster
      user: admin@k3d-mycluster
    name: k3d-mycluster
  - context:
      cluster: orbstack
      user: orbstack
    name: orbstack
users:
  - name: admin@k3d-mycluster
    user:
      client-certificate-data: abcdummy
      client-key-data: dummydata
  - name: orbstack
    user:
      client-certificate-data: dummydata
      client-key-data: dummydata
```


> k config view
