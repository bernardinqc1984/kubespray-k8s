1. **command 1 k8s kubespray**
   
```bash
sudo ETCDCTL_API=3 etcdctl [COMMANDE] \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem

```

2. **Command 2 - Afficher toutes les clée etcd**

```bash
sudo ETCDCTL_API=3 etcdctl get / --prefix --keys-only \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
```

3. **Command 3 - Afficher les membres etcd**

```bash
sudo ETCDCTL_API=3 etcdctl member list   --endpoints=https://172.16.1.20:2379   --cacert=/etc/ssl/etcd/ssl/ca.pem   --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem   --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
77014c5332cade09, started, etcd1, https://172.16.1.20:2380, https://172.16.1.20:2379, false
811a9aebf5e9d602, started, etcd2, https://172.16.1.21:2380, https://172.16.1.21:2379, false
e17fe110b367d079, started, etcd3, https://172.16.1.22:2380, https://172.16.1.22:2379, false

sudo ETCDCTL_API=3 etcdctl endpoint status --cluster -w table \
  --endpoints=https://172.16.1.20:2379,https://172.16.1.21:2379,https://172.16.1.22:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
|         ENDPOINT         |        ID        | VERSION | DB SIZE | IS LEADER | IS LEARNER | RAFT TERM | RAFT INDEX | RAFT APPLIED INDEX | ERRORS |
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
| https://172.16.1.20:2379 | 77014c5332cade09 |  3.5.21 |   58 MB |     false |      false |        10 |   25501899 |           25501899 |        |
| https://172.16.1.21:2379 | 811a9aebf5e9d602 |  3.5.21 |   54 MB |     false |      false |        10 |   25501899 |           25501899 |        |
| https://172.16.1.22:2379 | e17fe110b367d079 |  3.5.21 |   57 MB |      true |      false |        10 |   25501899 |           25501899 |        |
+--------------------------+------------------+---------------+--------------------------+------------------+---------+---------+-----------+------
```

4. **Compter le nombre total de clés**

```bash
sudo ETCDCTL_API=3 etcdctl get / --prefix --keys-only --endpoints=https://172.16.1.20:2379 --cacert=/etc/ssl/etcd/ssl/ca.pem --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem | wc -l
4484
``

5. **Voir les clés d'un namespace spécifique**

```bash
sudo ETCDCTL_API=3 etcdctl get /registry/pods/openbao --prefix --keys-only --endpoints=https://172.16.1.20:2379 --cacert=/etc/ssl/etcd/ssl/ca.pem --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
/registry/pods/openbao/openbao-0

/registry/pods/openbao/openbao-1

/registry/pods/openbao/openbao-2

/registry/pods/openbao/openbao-agent-injector-566f644c57-w9x69


sudo ETCDCTL_API=3 etcdctl get /registry/pods/monitoring --prefix --keys-
only --endpoints=https://172.16.1.20:2379 --cacert=/etc/ssl/etcd/ssl/ca.pem --cert=/etc/ssl/etcd/
ssl/admin-k8s-dev-1-cp01.pem --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
/registry/pods/monitoring/alertmanager-main-0

/registry/pods/monitoring/alertmanager-main-1

/registry/pods/monitoring/alertmanager-main-2

/registry/pods/monitoring/blackbox-exporter-d4d45d65-dqnh4

/registry/pods/monitoring/grafana-657bfc966-fhq6x

/registry/pods/monitoring/kube-state-metrics-6d8b8c455-ghfk4

/registry/pods/monitoring/node-exporter-44p4h

/registry/pods/monitoring/node-exporter-4qsj9

/registry/pods/monitoring/node-exporter-7wwtp

/registry/pods/monitoring/node-exporter-lkshs

/registry/pods/monitoring/node-exporter-q4wrq

/registry/pods/monitoring/node-exporter-tgr6x

/registry/pods/monitoring/prometheus-adapter-784f566c54-479rs

/registry/pods/monitoring/prometheus-adapter-784f566c54-p5tjq

/registry/pods/monitoring/prometheus-k8s-0

/registry/pods/monitoring/prometheus-k8s-1

/registry/pods/monitoring/prometheus-operator-f649bcd58-s4qq6
```

6. **Obtenir les détails d'une ressource spécifique**

```bash
sudo ETCDCTL_API=3 etcdctl get /registry/services/specs/openbao/openbao-active --endpoints=https://172.16.1.20:2379 --cacert=/etc/ssl/etcd/ssl/ca.pem --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
```

7. **Vérifier la santé de tous les endpoints**

```bash
sudo ETCDCTL_API=3 etcdctl endpoint health --cluster -w table \
  --endpoints=https://172.16.1.20:2379,https://172.16.1.21:2379,https://172.16.1.22:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
+--------------------------+--------+-------------+-------+
|         ENDPOINT         | HEALTH |    TOOK     | ERROR |
+--------------------------+--------+-------------+-------+
| https://172.16.1.22:2379 |   true |  11.13495ms |       |
| https://172.16.1.21:2379 |   true | 11.006589ms |       |
| https://172.16.1.20:2379 |   true | 14.703002ms |       |
+--------------------------+--------+-------------+-------+
```

8. **Voir les métriques de performance**

```bash
sudo ETCDCTL_API=3 etcdctl check perf \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
 59 / 60 Booooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooom !  98.33%PASS: Throughput is 151 writes/s
PASS: Slowest request took 0.327024s
PASS: Stddev is 0.014901s
PASS
```

9. **Lister tous les pods d'un namespace**

```bash
sudo ETCDCTL_API=3 etcdctl get /registry/pods/openbao --prefix --keys-only \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
/registry/pods/openbao/openbao-0

/registry/pods/openbao/openbao-1

/registry/pods/openbao/openbao-2

/registry/pods/openbao/openbao-agent-injector-566f644c57-w9x69
```

10. **Voir tous les namespaces**

```bash
sudo ETCDCTL_API=3 etcdctl get /registry/namespaces --prefix --keys-only \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
/registry/namespaces/cirrus-nfs

/registry/namespaces/default

/registry/namespaces/kube-node-lease

/registry/namespaces/kube-public

/registry/namespaces/kube-system

/registry/namespaces/monitoring

/registry/namespaces/observability

/registry/namespaces/openbao
```

11. **Lister tous les secrets**

```bash
$ sudo ETCDCTL_API=3 etcdctl get /registry/secrets --prefix --keys-only \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
/registry/secrets/monitoring/additional-scrape-configs

/registry/secrets/monitoring/alertmanager-main

/registry/secrets/monitoring/alertmanager-main-cluster-tls-config

/registry/secrets/monitoring/alertmanager-main-generated

/registry/secrets/monitoring/alertmanager-main-tls-assets-0

/registry/secrets/monitoring/alertmanager-main-web-config

/registry/secrets/monitoring/grafana-config

/registry/secrets/monitoring/grafana-credentials

/registry/secrets/monitoring/grafana-datasources

/registry/secrets/monitoring/kube-etcd-client-certs

/registry/secrets/monitoring/prometheus-k8s

/registry/secrets/monitoring/prometheus-k8s-thanos-prometheus-http-client-file

/registry/secrets/monitoring/prometheus-k8s-tls-assets-0

/registry/secrets/monitoring/prometheus-k8s-web-config

/registry/secrets/openbao/openbao-server-tls

/registry/secrets/openbao/sh.helm.release.v1.openbao.v1
```

Défragmenter la base de données etcd

sudo ETCDCTL_API=3 etcdctl defrag --cluster \
  --endpoints=https://172.16.1.20:2379,https://172.16.1.21:2379,https://172.16.1.22:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem


12. **Compacter l'historique (libérer de l'espace)**

#### D'abord, obtenir la révision actuelle

```bash
sudo ETCDCTL_API=3 etcdctl endpoint status --write-out="table" \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
|         ENDPOINT         |        ID        | VERSION | DB SIZE | IS LEADER | IS LEARNER | RAFT TERM | RAFT INDEX | RAFT APPLIED INDEX | ERRORS |
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
| https://172.16.1.20:2379 | 77014c5332cade09 |  3.5.21 |   58 MB |     false |      false |        10 |   25514628 |           25514628 |        |
+--------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
```

# Puis compacter (remplacer XXXXX par la révision)

```bash
sudo ETCDCTL_API=3 etcdctl compaction XXXXX \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
```

13. **Créer un snapshot de sauvegarde**

```bash
sudo ETCDCTL_API=3 etcdctl snapshot save /tmp/etcd-backup-$(date +%Y%m%d-%H%M%S).db \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
```

14. **Vérifier un snapshot**

```bash
sudo ETCDCTL_API=3 etcdctl snapshot status /tmp/etcd-backup-*.db -w table
Deprecated: Use `etcdutl snapshot status` instead.

+---------+----------+------------+------------+
|  HASH   | REVISION | TOTAL KEYS | TOTAL SIZE |
+---------+----------+------------+------------+
| 56f42c6 | 21421074 |       1931 |      58 MB |
+---------+----------+------------+------------+
```

14. or use the commande below, the fire one is deprecated

```bash
sudo ETCDCTL_API=3 etcdutl snapshot status /tmp/etcd-backup-*.db -w table
+---------+----------+------------+------------+
|  HASH   | REVISION | TOTAL KEYS | TOTAL SIZE |
+---------+----------+------------+------------+
| 56f42c6 | 21421074 |       1931 |      58 MB |
+---------+----------+------------+------------+
```

15. **Rechercher des clés par pattern**

```bash
• # Tous les ConfigMaps

sudo ETCDCTL_API=3 etcdctl get /registry/configmaps --prefix --keys-only \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem

• # Voir les alarmes actives

sudo ETCDCTL_API=3 etcdctl alarm list \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
```

16. **Surveillance en temps réel (watch)**

```bash
# Surveiller les changements sur les pods openbao
sudo ETCDCTL_API=3 etcdctl watch /registry/pods/openbao --prefix \
  --endpoints=https://172.16.1.20:2379 \
  --cacert=/etc/ssl/etcd/ssl/ca.pem \
  --cert=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01.pem \
  --key=/etc/ssl/etcd/ssl/admin-k8s-dev-1-cp01-key.pem
```
