# How to install Livy
```bash
$ helm install livy ./charts/livy
$ helm uninstall livy
$ helm list

$ kubectl get pod

# Livy Session Mode
$ kubectl exec --namespace default livy-0 -- \
    wget -qO- --header='Content-Type: application/json' --method=POST \
      --body-data='{
            "name": "session-01",
            "numExecutors": 2,
            "conf": {
                "spark.kubernetes.namespace": "default",
                "spark.kubernetes.authenticate.driver.serviceAccountName": "spark"
            }
          }' "http://localhost:8998/sessions" 

$ kubectl exec --namespace default livy-0 -- \
    wget -qO- --header='Content-Type: application/json' --method=POST \
      --body-data='{
            "kind": "spark",
            "code": "1+1"
          }' "http://localhost:8998/sessions/0/statements"

$ kubectl exec --namespace default livy-0 -- \
    wget -qO- --method=DELETE \
      "http://localhost:8998/sessions/0"            

# Livy Batch Mode
$ kubectl exec --namespace default livy-0 -- \
    wget -qO- --header='Content-Type: application/json' --method=POST \
      --body-data='{
            "name": "batch-01",
            "className": "org.apache.spark.examples.SparkPi",
            "numExecutors": 2,
            "file": "local:///opt/spark/examples/jars/spark-examples_2.12-3.0.1.jar",
            "args": ["10"],
            "conf": {
                "spark.kubernetes.namespace": "default"
            }
          }' "http://localhost:8998/batches" 

$ kubectl exec --namespace default livy-0 -- \
    wget -qO- --method=DELETE \
    "http://localhost:8998/batches/0"
```

# How to install spark-cluster
```bash
$ ./charts/spark-cluster/helm dependency update
$ helm upgrade spark-cluster ./charts/spark-cluster --install
$ helm uninstall spark-cluster
$ helm list

$ kubectl get pod
$ kubectl delete pod <pod name>
```