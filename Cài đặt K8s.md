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
kubectl delete pod <pods name>
```

Xoá tất cả pods trong một namespace
```
kubectl delete pods --all --grace-period=0 --force --namespace namespace
```
