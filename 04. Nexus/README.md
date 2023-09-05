# The below Steps to Install Jenkins using Helm and Enable Ingress

### 1. switch to Nexus namespace
```console
kubectl config set-context --current --namespace=nexus
```

### 2. Create Storage Class, PV and PVC for Nexus
- You can check the files [nexus-sc.yaml](https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/01.%20Nexus-sc.yaml), [nexus-pv.yaml](https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/02.%20Nexus-pv.yaml), [nexus-pvc.yaml](https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/03.%20Nexus-pvc.yaml)
```console
kubectl apply -f nexus-sc.yaml
```
```console
kubectl apply -f nexus-pv.yaml
```
```console
kubectl apply -f nexus-pvc.yaml
```
### 3. Create nexus-values.yaml
- You can check [nexus-values.yaml](https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/04.%20Nexus-Values.yaml) files in the same directory
  
### 4. Install Nexus OSS3 using helm
```console
helm repo add sonatype https://sonatype.github.io/helm3-charts
```
```console
helm repo update
```
```console
helm install nexus sonatype/nexus-repository-manager -f nexus-values.yaml --namespace nexus
```

### 5. Expose the Nexus service to access
```console
kubectl port-forward service/nexus-nexus-repository-manager 8081:8081 --namespace nexus
```

### 6. Configure Ingress to be accessable outside
- You can check [nexus-ingress.yaml](https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/05.%20Nexus-ingress.yaml) file in the same directory
- Add in the hosts file the worker node public IP and the hostname
```console
kubectl apply -f nexus-ingress.yaml
```
```console
curl -kv -H "Host: nexus.atostask.com" http://"workernodeprivateip
```

### 7. get the admin password
```console
kubectl exec --stdin --tty nexus-nexus-repository-manager-5cdf557cb5-ncgnp  -- /bin/bash
```
```console
cat /nexus-data/admin.password
```
### Troubleshooting Tips:
- Make sure that you give a right permission to the local volume files
```console
sudo chmod 777 -R /data/nexux/
```
