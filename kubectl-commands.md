```bash
kubectl -n efk2 set resources daemonset fluentd --limits=memory=1Gi,cpu=500m --requests=memory=512Mi,cpu=250m
```

```bash
kubectl -n efk2 get pods -l app.kubernetes.io/name=fluentd
```

```bash
kubectl -n efk2 delete all,cm,configmap,serviceaccount -l app.kubernetes.io/name=kibana --force --grace-period=0
```

#### Elasticsearch

```bash
kubectl -n efk2 exec elasticsearch-master-0 -- bash -c 'curl -s -u elastic:MTzB7do6bSgLR0rn -k https://localhost:9200/_cat/indices?v'
````

#### Kibana

```bash
kubectl delete serviceaccount pre-install-kibana-kibana -n efk2 && kubectl delete role pre-install-kibana-kibana post-delete-kibana-kibana -n efk2 && kubectl delete rolebinding post-delete-kibana-kibana -n efk2 && kubectl delete secret kibana-kibana-es-token pre-install-kibana-kibana-token-75lwp -n efk2
```

#### Loki

1. 
```bash
helm uninstall loki -n monitoring && sleep 5 && kubectl -n monitoring delete pvc --all -l release=loki 2>&1 | head -10
```

2.
```bash
kubectl -n monitoring get pods -l app=promtail --no-headers | head -10
```

3.
```bash
kubectl -n monitoring port-forward svc/grafana 3000:3000 &>/dev/null &
```

4.
```bash
pkill -f "port-forward svc/grafana"
```

```bash
helm install loki grafana/loki-stack \
  --version 2.10.3 \
  -n monitoring \
  --set grafana.enabled=false \
  --set loki.persistence.storageClassName=cirrusprod-nfs-storage \
  --set loki.persistence.size=50Gi \
  --set 'loki.config.schema_config.configs[0].store=loki' \
  --set 'loki.config.storage_config.filesystem.directory=/loki' 2>&1 | tail -15
```

#### Loki Retention

1. 
```bash
kubectl -n monitoring get secret loki -o jsonpath='{.data.loki\.yaml}' 2>/dev/null | base64 -d
```

2.
```bash
kubectl -n monitoring get secret loki -o jsonpath='{.data.loki\.yaml}' 2>/dev/null | base64 -d | grep -A 20 "limits_config:"
```
