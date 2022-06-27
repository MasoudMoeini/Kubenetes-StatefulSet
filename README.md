# Kubenetes StatefulSet
Like a Deployment, a StatefulSet manages Pods that are based on an identical container spec<br>
Unlike a Deployment, a StatefulSet maintains a sticky identity for each of their Pods<br>
These pods are created from the same spec, but are not interchangeable: <br>
each has a persistent identifier that it maintains across any rescheduling.[Reference](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)<br>