### Patch Calico
```bash
kubectl patch felixconfiguration default -n kube-system --type='merge' -p '{"spec":{"bpfConnectTimeLoadBalancing":"Disabled"}}'
```

### Debug réseau avec Pod

1.
```bash
# Pod avec des outils réseau complets (dnsutils)
kubectl run debug-pod --image=nicolaka/netshoot --rm -it -- /bin/bash

# Une fois dans le pod, vous pouvez utiliser :
nslookup www.projectcharles.cloud
ping www.projectcharles.cloud
dig www.projectcharles.cloud
curl http://www.projectcharles.cloud
hostprojectcharles.cloud
```
2.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: debug-network
  namespace: default
  labels:
    app: debug
spec:
  containers:
  - name: netshoot
    image: nicolaka/netshoot
    command:
      - sleep
      - "infinity"
    resources:
      limits:
        cpu: "500m"
        memory: "512Mi"
      requests:
        cpu: "100m"
        memory: "128Mi"
  restartPolicy: Always
```

3.
```bash
kubectl run -it --rm debug --image=busybox --restart=Never -- nslookup www.projectcharles.cloud

# Test ping rapide
kubectl run -it --rm debug --image=busybox --restart=Never -- ping -c 4 www.projectcharles.cloud

# Test avec netshoot (tous les outils)
kubectl run -it --rm debug --image=nicolaka/netshoot --restart=Never -- nslookup gwww.projectcharles.cloud
```

### Update DNS

```bash
inventory/k8scluster/group_vars/k8s_cluster/k8s-cluster.yml
```

```bash
#   - name website.tld website.namespace.svc.cluster.local
dns_etchosts: |
  1.1.5.8 www.projectcharles.cloud
  1.12.25.1 ident.cluster0.charles.cloud
  1.21.55.1 ident.cluster1.charles.cloud
  1.11.25.3 ident.cluster2.charles.cloud
  1.21.25.4 ident.cluster3.charles.cloud
  1.11.4.6 mail.cluster.charles.cloud
  1.11.5.11 server1.cluster.charles.cloud
```

2. **Ajoutez la variable** dns_etchosts
Ouvrez le fichier et cherchez la section DNS (autour de la ligne 160-210). Ajoutez cette configuration :

```bash
# Enable nodelocal dns cache
enable_nodelocaldns: true
enable_nodelocaldns_secondary: false
nodelocaldns_ip: 169.254.25.10
nodelocaldns_health_port: 9254
nodelocaldns_second_health_port: 9256
nodelocaldns_bind_metrics_host_ip: false
nodelocaldns_secondary_skew_seconds: 5

#   - name website.tld website.namespace.svc.cluster.local
# ========================================
# Hosts personnalisés pour DNS
# ========================================
dns_etchosts: |
  1.1.5.8 www.projectcharles.cloud
  1.12.25.1 ident.cluster0.charles.cloud
  1.11.4.6 mail.cluster.charles.cloud
  1.11.5.11 server1.cluster.charles.cloud
```

3. **Déployez ou mettez à jour votre cluster**
   
```bash
# Pour un nouveau déploiement complet
ansible-playbook -i inventory/local/hosts.ini cluster.yml

# OU pour mettre à jour uniquement NodeLocalDNS et CoreDNS
ansible-playbook -i inventory/local/hosts.ini cluster.yml --tags dns,nodelocaldns

kubectl get configmap -n kube-system nodelocaldns -o yaml
```

```bash
# 1. Créer un environnement virtuel avec Python 3.12
python3.12 -m venv kubespray-venv

# 2. Activer l'environnement virtuel
source kubespray-venv/bin/activate
deactivate

# 3. Mettre à jour pip dans l'environnement virtuel
pip install --upgrade pip

# 4. Installer les requirements
pip install -r requirements.txt

# 5. Vérifier la version d'Ansible installée
ansible --version

# 6. Lancer votre déploiement Kubespray
ansible-playbook -i inventory/k8scluster/inventory.ini --become --become-user=root cluster.yml -vvv
ansible-playbook upgrade-cluster.yml -b -i inventory/k8scluster/inventory.ini -e kube_version=1.31.10 -vvv

ansible-playbook -i inventory/k8scluster/inventory.ini --become --become-user=root cluster.yml --tags metrics_server -vvv

ansible-playbook -i inventory/k8scluster/inventory.ini --become --become-user=root cluster.yml -e tags=nodelocaldns -vv

# Votre exemple original
ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml -t nodelocaldns

# Autres exemples
ansible-playbook -i inventory/k8scluster/inventory.ini --become --become-user=root cluster.yml -e tags=preinstall
ansible-playbook -i inventory/k8scluster/inventory.ini --become --become-user=root cluster.yml -e tags=container-engine
ansible-playbook -i inventory/k8scluster/inventory.ini --become --become-user=root cluster.yml -e tags=network
ansible-playbook -i inventory/k8scluster/inventory.ini --become --become-user=root cluster.yml -e tags=node,control-plane

# 7. Limiter l'installation ou l'upgrade à un ou plusieurs serversure
ansible-playbook upgrade-cluster.yml -b -i inventory/k8scluster/inventory.ini --limit master02 -vvv
ansible-playbook upgrade-cluster.yml -b -i inventory/k8scluster/inventory.ini --limit "worker02:worker03" -vvv
```
