#### History
```bash
helm history loki -n monitoring
```
```bash
REVISION        UPDATED                         STATUS          CHART                   APP VERSION     DESCRIPTION     
1               Fri Feb 20 09:10:36 2026        superseded      loki-stack-2.10.3       v2.9.3          Install complete
2               Fri Feb 20 11:00:49 2026        superseded      loki-stack-2.10.3       v2.9.3          Upgrade complete
3               Fri Feb 20 11:03:01 2026        superseded      loki-stack-2.10.3       v2.9.3          Upgrade complete
4               Fri Feb 20 11:07:07 2026        superseded      loki-stack-2.10.3       v2.9.3          Rollback to 2   
5               Fri Feb 20 11:09:41 2026        superseded      loki-stack-2.10.3       v2.9.3          Upgrade complete
6               Fri Feb 20 11:10:44 2026        deployed        loki-stack-2.10.3       v2.9.3          Rollback to 1
```

#### Helm roolback
```bash
helm rollback loki 1 -n monitoring
```

