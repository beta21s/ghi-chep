Hướng dẫn cài đặt K8s https://github.com/beta21s/ghichep-kubernetes


#### Một số lệnh trên K8s

kubectl run test-nginx --image=nginx --port=80

Lấy tất cả namespace
```
kubectl get pods --all-namespaces
```

Lấy tất cả pod
```
kubectl get pods
kubectl describe pod nginx
kubectl get pods --all-namespaces
```

Xoá một pods
```
  <pods name>
```

Xoá tất cả pods trong một namespace ```<name>```
```
kubectl delete --all deployments --namespace=<name>
```

Lấy thông tin chi tiết một services
```
kubectl -n kubernetes-dashboard get service kubernetes-dashboard
```

Triển khai ứng dg trên template
```
kubectl delete -f recommended.yaml
```

Tạo tolken kubernetes-dashboard
```
kubectl -n kubernetes-dashboard create token admin-user
```

Thực thi lệnh bash trên container trên K8s
```
kubectl exec -ti --namespace <namespace> <pod-name>  bash
```

Xem cấu hình k8s cluster
```
kubectl config view
```

Lấy SSL của K8s
```
echo -n|openssl s_client -connect 172.20.8.10:6443 |openssl x509 -outform PEM > selfsigned_certificate.pem
```

Chạy Spark trên K8s
```
spark-submit  \
    --master k8s://https://172.20.8.10:6443  \
    --deploy-mode cluster  \
    --conf spark.executor.instances=5  \
    --conf spark.kubernetes.container.image=truong96/spark-k8s:tagname  \
    --class org.apache.spark.examples.SparkPi  \
    --conf spark.kubernetes.authenticate.submission.oauthToken= <tolken> \
    --conf spark.kubernetes.authenticate.submission.caCertFile= <path.pem> \
    --name spark-pi  \
    local:///opt/spark/examples/jars/spark-examples_2.12-3.0.1.jar
```
