# apiportal-on-k8s
Just to document how to run API-Portal on Kubernetes

A sample deployment descriptor:
```
apiVersion: v1
kind: Service
metadata:
  name: apiportal-service
spec:
  type: LoadBalancer
  selector:
    app: apiportal
  ports:
    - protocol: TCP
      port: 443
      targetPort: 443
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: apiportal
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: apiportal
  template:
    metadata:
      labels:
        app: apiportal
    spec:
      containers:
      - name: apiportal
        image: 268547697226.dkr.ecr.us-east-1.amazonaws.com/cwiechmann/axway:latest
        env:
        - name: MYSQL_HOST
          value: apiportal-database-1.chmbveww115t.us-east-1.rds.amazonaws.com
        - name: MYSQL_PORT
          value: "3306"
        - name: MYSQL_ROOT_PASSWORD
          value: changeme
        - name: MYSQL_USERNAME
          value: joomla
        - name: MYSQL_PASSWORD
          value: changeme
        - name: MYSQL_DBNAME
          value: joomla
        - name: APIMANAGER_HOST
          value: "127.0.0.1"
        - name: APIMANAGER_PORT
          value: "8075"
        ports:
        - containerPort: 443
          name: apiportal-port
      imagePullSecrets:
      - name: harbor-secret

```
