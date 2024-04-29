## Create Deployment
Deployment can be created by using 'kubectl create deployment' command: 
``` bash
kubectl create deployment task6-1 --image=chunyiwang/workshop:v6 --dry-run=client -o yaml > task6-1.yaml #--dry-run=client: Verify configuration without making actual changes
```
Deployment: 
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels: # label: the label of a resource, which can be used to filter resources
    app: task6-1
  name: task6-1
spec:
  replicas: 1
  selector: # selector: select a set of labels that define which Pods belong to this deployment.
    matchLabels:
      app: task6-1
  strategy: {} #strategy: Deployment strategy, null value means using the default strategy of rolling updates
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: task6-1
    spec:
      containers:
      - image: chunyiwang/workshop:v6
        name: workshop
        resources: {} #Define resource requests and limitations (such as memory or CPU), where null means unlimited
status: {}
```
## Create Service
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: task6-1-service
spec:
  type: ClusterIP # The type of Service is to create a virtual IP
  clusterIP: 10.1.122.2 # Address of virtual IP
  selector:
    app: task6-1
  ports:
    - port: 80 # Exposed port
      targetPort: 3040 # The actual port for accepting request and resolution (the port defined in the code)
      protocol: TCP # Type of network protocol
```

## Apply the configuration
``` bash
kubectl apply -f task6-1.yaml
```

## Test
Accessing the homepage
``` bash
curl http://10.1.122.2
```
Test the function of adding two numbers
``` bash
curl 'http://10.1.122.2?n1=5&n2=10'
```
