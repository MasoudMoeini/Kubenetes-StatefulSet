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
Examin the pods running in Ordinal index
```
kubectl get pods -l app=nginx
```
The Pods' names take the form <statefulset name>-<ordinal index>.  <br>
Since the web StatefulSet has two replicas, it creates two Pods, web-0 and web-1.<br>
In the StatefulSets concept, the Pods in a StatefulSet have a sticky, unique identity.<br>
This identity is based on a unique ordinal index that is assigned to each Pod by the StatefulSet controller.<br>
Each pod has stable host name based on its ordinal index.<br>
As each Pod can incorporate several containers, kubectl exec supports an additional argument<br>
to let you specify a Pod and container to connect to:<br>
**kubectl exec -it <pod> -c <container> -- /bin/sh**  <br>
kubectl exec for deployment:<br>
kubectl exec -it deployment/demo-deployment -- /bin/sh<br>
start a shell session inside containers running in your Kubernetes cluster
```
for i in 0 1; do kubectl exec "web-$i" -- sh -c 'hostname'; done
```
Using nslookup on the Pods' hostnames, you can examine their in-cluster DNS addresses:
```
kubectl run -i --tty --image busybox:1.28 dns-test --restart=Never --rm
nslookup web-0.nginx
nslookup web-1.nginx
```
When you deleting all StatefulSet's Pods, StatefulSet Controller create new pods wuth the same <br>
hostname but different dns addresses
```
kubectl delete pod -l app=nginx
```
Running again 
```
kubectl get pods -l app=nginx
```
Pods automatically again created with the same id and different addresses 
```
kubectl get pod -w -l app=nginx
kubectl run -i --tty --image busybox:1.28 dns-test --restart=Never --rm /bin/sh
nslookup web-0.nginx
```
# Persistent Volume  
```
kubectl get pvc -l app=nginx
``` 
```
for i in 0 1; do kubectl exec "web-$i" -- sh -c 'echo "$(hostname)" > /usr/share/nginx/html/index.html'; done
for i in 0 1; do kubectl exec -i -t "web-$i" -- curl http://localhost/; done
```
exception: if image does not have curl RUN:
```
kubectl port-forward <pod-name eg web-0> 8080:80
http localhost:80
curl http://localhost:80/
```  

Scaling a StatefulSet
```
kubectl get pods -w -l app=nginx
kubectl scale sts web --replicas=5
kubectl get pods -w -l app=nginx
kubectl patch sts web -p '{"spec":{"replicas":3}}'
kubectl get pods -w -l app=nginx
```
Switch bach to pvc
```
kubectl get pvc -l app=nginx
```
The PersistentVolumes mounted to the Pods of a StatefulSet are not deleted 
when the StatefulSet's Pods are deleted<br>
Get the Pods to view their container images:
```
for p in 0 1 2; do kubectl get pod "web-$p" --template '{{range $i, $c := .spec.containers}}{{$c.image}}{{end}}'; echo; done
```
To delete Statefulset without deleting pods
```
kubectl delete statefulset web --cascade=orphan
kubectl get pods -l app=nginx
```
Delete resources
```
kubectl delete service nginx
kubectl delete statefulset web
```