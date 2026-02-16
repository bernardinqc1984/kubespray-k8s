1. **Grafana secret (credentials**
   
```yaml
cat grafana-credentials.yaml 
apiVersion: v1
data:
  password: YWRtaW5fY2lycnVz
  username: YWRtaW4=
kind: Secret
metadata:
  name: grafana-credentials
  namespace: monitoring
type: Opaque


spec:
      automountServiceAccountToken: false
      containers:
      - env:
        - name: GF_SECURITY_ADMIN_USER
          valueFrom:
            secretKeyRef:
              key: username
              name: grafana-credentials
        - name: GF_SECURITY_ADMIN_PASSWORD
          valueFrom:
            secretKeyRef:
              key: password
              name: grafana-credentials
        image: grafana/grafana:12.0.1
```

2. **Grafana custom Dashboard
   
```yaml
apiVersion: v1
data:
  dashboards.yaml: |-
    {
        "apiVersion": 1,
        "providers": [
            {
                "folder": "Default",
                "folderUid": "",
                "name": "0",
                "options": {
                    "path": "/grafana-dashboard-definitions/0"
                },
                {
                "folder": "custom-dashboard",
                "folderUid": "",
                "name": "1",
                "options": {
                    "path": "/grafana-dashboard-definitions/1"
                },
                "orgId": 1,
                "type": "file"
            }
        ]
    }
kind: ConfigMap
metadata:
  labels:
    app.kubernetes.io/component: grafana
    app.kubernetes.io/name: grafana
    app.kubernetes.io/part-of: kube-prometheus
    app.kubernetes.io/version: 12.0.1
  name: grafana-dashboards
  namespace: monitoring
```
