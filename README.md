# Multi-Container Pods in Kubernetes
Simple tutorial to demonstrate the concept of packaging multiple containers into a single pod. 
* Web Pod has a Python Flask container and a Redis container
* DB Pod has a MySQL container
* When data is retrieved through the Python REST API, it first checks within Redis cache before accessing MySQL
* Each time data is fetched from MySQL, it gets cached in the Redis container of the same Pod as the Python Flask container


## Deploy the app to Kubernetes
```
cd ../Deploy
kubectl create -f db-pod.yml
kubectl create -f db-svc.yml
kubectl create -f web-pod-1.yml
kubectl create -f web-svc.yml
```

## Check that the Pods and Services are created
```
kubectl get pods
kubectl get svc
```

## Get the IP address of one of the Nodes and the NodePort for the web Service. Populate the variables with the appropriate values
```
kubectl get nodes
kubectl describe svc web

kubectl get nodes
export NODE_IP=<NODE_IP>
export NODE_PORT=<NODE_PORT>
```

## Initialize the database with sample schema
```
curl http://$NODE_IP:$NODE_PORT/init
```
## Insert some sample data
```
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "1", "user":"John Doe"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "2", "user":"Jane Doe"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "3", "user":"Bill Collins"}' http://$NODE_IP:$NODE_PORT/users/add
curl -i -H "Content-Type: application/json" -X POST -d '{"uid": "4", "user":"Mike Taylor"}' http://$NODE_IP:$NODE_PORT/users/add
```

## Access the data 
```
curl http://$NODE_IP:$NODE_PORT/users/1
```


