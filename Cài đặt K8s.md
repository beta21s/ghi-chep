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

