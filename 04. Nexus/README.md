# The below Steps to Install Jenkins using Helm and Enable Ingress

### 1. switch to Nexus namespace
```console
kubectl config set-context --current --namespace=nexus
```

### 2. Create Storage Class, PV and PVC for Nexus
- You can check the files (nexus-sc.yaml)[https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/01.%20Nexus-sc.yaml], (nexus-pv.yaml)[https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/02.%20Nexus-pv.yaml], (nexus-pvc.yaml)[https://github.com/davabdallah/Atos-Task/blob/main/04.%20Nexus/03.%20Nexus-pvc.yaml]

3. Create values.yaml and jenkins-values.yaml
Refert to nexus-values.yaml file in the same directory

4. Install Nexus OSS3 using helm
helm repo add sonatype https://sonatype.github.io/helm3-charts
helm repo update
helm install nexus sonatype/nexus-repository-manager -f nexus-values.yaml --namespace nexus

5. Expose the Nexus service to access
kubectl port-forward service/nexus-nexus-repository-manager 8081:8081 --namespace nexus

6. Configure Ingress to be accessable outside
kubectl apply -f nexus-ingress.yaml
Refert to nexus-ingress.yaml file in the same directory
# Add in hostes file the workernode public IP and the host name
# curl -kv -H "Host: nexus.atostask.com" http://"workernodeprivateip
7. get the admin password
kubectl exec --stdin --tty nexus-nexus-repository-manager-5cdf557cb5-ncgnp  -- /bin/bash
cat /nexus-data/admin.password 
Troubleshooting Tips:
    # Make sure that you give a right permission to the local volume files
      sudo chmod 777 -R /data/nexux/
