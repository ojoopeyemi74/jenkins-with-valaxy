# once we have our cluster setup and installed kubctl and eksctl
- next: we are creating a deployment using our already built Docker image which was on Dockerhub

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: opeyemi-regapp
  labels:
    app: regapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: regapp
  template:
    metadata:
      labels:
        app: regapp
    spec:
      containers:
      - name: regapp
        image: opeyemiojo/regapps:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1

---
apiVersion: v1
kind: Service
metadata:
  name: regapp
spec:
  selector:
    app: regapp
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer

```

# and we should be able to access our Application /webapp 
example:  http://a1f9beac93cf54cb18dd1065ac1edfaf-1135621804.eu-west-2.elb.amazonaws.com/webapp/

# with this done if any of our pod get terminated another pod will come up automatically

