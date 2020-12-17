#### Custom Scheduler

We can run N number of schedulers in K8s clsuter. Here, is a simple custom scheduler example, we can get the config manifest from K8s docs page.

Refer the current directory for `my-scheduler.yaml` config, it will create service account, clusterrole, clusterrolebinding and deployment for creating a scheduler, respectively.

```
controlplane $ k create -f my-scheduler.yaml
pod/my-scheduler created
```

```
controlplane $ k get po -n kube-system
NAME                                   READY   STATUS    RESTARTS   AGE
coredns-f9fd979d6-9chw8                1/1     Running   0          19m
coredns-f9fd979d6-hkdnm                1/1     Running   0          19m
etcd-controlplane                      1/1     Running   0          20m
kube-apiserver-controlplane            1/1     Running   0          20m
kube-controller-manager-controlplane   1/1     Running   0          20m
kube-flannel-ds-amd64-7cpqg            1/1     Running   0          19m
kube-flannel-ds-amd64-jpb26            1/1     Running   0          19m
kube-proxy-2s4p7                       1/1     Running   0          19m
kube-proxy-mtm88                       1/1     Running   0          19m
kube-scheduler-controlplane            1/1     Running   0          20m
my-scheduler-6fdc57459c-279tn          1/1     Running   0          8m1s
```

If RBAC is enabled on your cluster, you must update the system:kube-scheduler cluster role. Add your scheduler name to the resourceNames of the rule applied for endpoints and leases resources, as in the following example:

```
kubectl edit clusterrole system:kube-scheduler
```

Deploy the application in pod using `pod.yaml` and `nginx-pod.yaml`. Once, those are deployed, we can check the description about those pods. In event, we can see the name of scheduler as `my-scheduler` who scheduled the pod rahter than the kubernetes default scheduler.

We can also check using command,

```
kubectl get events
```