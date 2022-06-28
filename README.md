# Kubenetes StatefulSet
Like a Deployment, a StatefulSet manages Pods that are based on an identical container spec<br>
Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods<br>
These pods are created from the same spec, but are not interchangeable:[Reference](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)<br>
```
kubectl get pods -w -l app=nginx
```
```
kubectl apply -f web.yaml
```
```
kubectl get service nginx
```
```
kubectl get statefulset web
```
```
kubectl get pods -w -l app=nginx
```
```
kubectl get pods -l app=nginx
```
Each pod has stable host name 
```
for i in 0 1; do kubectl exec "web-$i" -- sh -c 'hostname'; done
```
Create dns , with name of test
```
kubectl run -i --tty --image busybox:1.28 dns-test --restart=Never --rm
```
```
nslookup web-0.nginx
nslookup web-1.nginx
```
Stable Storage 
```
kubectl get pvc -l app=nginx
for i in 0 1; do kubectl exec "web-$i" -- sh -c 'echo "$(hostname)" > /usr/share/nginx/html/index.html'; done
for i in 0 1; do kubectl exec -i -t "web-$i" -- curl http://localhost/; done
```