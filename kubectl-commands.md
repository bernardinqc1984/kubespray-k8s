```bash
kubectl -n efk2 set resources daemonset fluentd --limits=memory=1Gi,cpu=500m --requests=memory=512Mi,cpu=250m
```

```bash
kubectl -n efk2 get pods -l app.kubernetes.io/name=fluentd
```

```bash
kubectl -n efk2 delete all,cm,configmap,serviceaccount -l app.kubernetes.io/name=kibana --force --grace-period=0
```
