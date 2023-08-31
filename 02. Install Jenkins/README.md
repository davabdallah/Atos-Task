# The below Steps to Install Jenkins using Helm and Enable Ingress

### 1. Switch to the Jenkins namespace

      
```console
kubectl config set-context --current --namespace=jenkins
```

### 2. Create Storage Class, PV and PVC for Jenkins
- Refer to jenkins-sc.yaml, jenkins-PV.yaml, Jenkins-PVC.yaml files in the same directory
- jenkins-sc.yaml
  ```yaml
  apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: jenkins-sc
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```

### 3. Create values.yaml and jenkins-casc.yaml
- Refer to values yaml files in the same directory

### 4. Install Jenkins using helm

```console 
      helm repo add Jenkins https://charts.jenkins.io
      helm repo update
      helm install jenkins jenkins/jenkins -f jenkins-values.yaml --namespace jenkins
```

### 5. Expose the Jenkins service to access
      kubectl port-forward service/jenkins 8080:8080 --namespace jenkins

### 6. Configure Ingress to be accessible outside
- Refer to jenkins-ingress.yaml file in the same directory
- Add in the hosts file the worker node public IP and the hostname
      curl -kv -H "Host: jenkins.atostask.com" http://"workernodeprivateip		

### 7. Retrieve the initial admin password  
      kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo

#### Troubleshooting Tips:

- Make Sure All Instances in the same VPC and can connect to each other
  
- Make sure that you give the correct permission to the config file 

 
      sudo chmod 644 /var/lib/kubelet/config.yaml

- If you face any errors you get the logs and the description of the failed pods

       kubectl logs jenkins-0 -c init -n Jenkins
- and
  
       kubectl decribe pods jenkins-0