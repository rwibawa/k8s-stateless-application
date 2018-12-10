# guestbook: stateless-application
[Tutorial](https://kubernetes.io/docs/tutorials/stateless-application/guestbook/)

## Commands:
```bash
$ kubectl apply -f redis-master-deployment.yaml
$ kubectl get pods
NAME                                   READY     STATUS    RESTARTS   AGE
redis-master-55db5f7567-vwqzx          1/1       Running   0          2m

$ kubectl logs -f redis-master-55db5f7567-vwqzx

$ kubectl apply -f redis-master-service.yaml
$ kubectl get service
NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
redis-master          ClusterIP      10.96.52.131     <none>        6379/TCP         54s

$ kubectl apply -f redis-slave-deployment.yaml
$ kubectl get pods
NAME                                   READY     STATUS              RESTARTS   AGE
redis-master-55db5f7567-vwqzx          1/1       Running             0          8m
redis-slave-584c66c5b5-ktwjd           0/1       ContainerCreating   0          9s
redis-slave-584c66c5b5-w9kcv           0/1       ContainerCreating   0          9s

$ kubectl apply -f redis-slave-service.yaml
$ kubectl get services
NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
redis-master          ClusterIP      10.96.52.131     <none>        6379/TCP         5m
redis-slave           ClusterIP      10.103.188.165   <none>        6379/TCP         9s

$ kubectl apply -f frontend-deployment.yaml
$ kubectl get pods -l app=guestbook -l tier=frontend
NAME                        READY     STATUS    RESTARTS   AGE
frontend-5c548f4769-cbvcl   1/1       Running   0          2m
frontend-5c548f4769-dxdlz   1/1       Running   0          2m
frontend-5c548f4769-z6m47   1/1       Running   0          2m

$ kubectl apply -f frontend-service.yaml
$ kubectl get services
NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
frontend              NodePort       10.106.110.88    <none>        80:31798/TCP     10s
redis-master          ClusterIP      10.96.52.131     <none>        6379/TCP         44m
redis-slave           ClusterIP      10.103.188.165   <none>        6379/TCP         39m

$ kubectl scale deployment frontend --replicas=5

# Clean up
$ kubectl delete deployment -l app=redis
$ kubectl delete service -l app=redis
$ kubectl delete deployment -l app=guestbook
$ kubectl delete service -l app=guestbook
```
