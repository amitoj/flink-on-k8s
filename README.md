# flink-on-k8s

## Cluster Setup
```
$ kubectl create -f flink-configuration-configmap.yaml
$ kubectl create -f jobmanager-service.yaml
$ kubectl create -f jobmanager-deployment.yaml
$ kubectl create -f taskmanager-deployment.yaml
```

## Access to Flink Web UI using Port Forwarding
```
$ kubectl port-forward $(kubectl get pods | grep flink-jobmanager | awk '{ print $1 }') 8081:8081
```

## Access to Flink Web UI via NodePort service on the rest service of jobmanager
```
$ kubectl create -f jobmanager-rest-service.yaml
```

## Deploy sample application
Clone flink repository and build.
```
$ git clone https://github.com/apache/flink.git
$ cd flink
$ git checkout -b release-1.9.2 refs/tags/release-1.9.2
$ mvn clean package -DskipTests
```

```
# if you use port forwarding to access job manager
$ ./build-target/bin/flink run -m localhost:8081 build-target/examples/streaming/WordCount.jar

# if you use NodePort service to access job manager
$ ./build-target/bin/flink run -m <public-node-ip>:<node-port> build-target/examples/streaming/WordCount.jar
```
