# The below Steps to Install Jenkins using Helm and Enable Ingress

### 1. Switch to the Jenkins namespace     
```console
kubectl config set-context --current --namespace=jenkins
```
### 2. Create Storage Class, PV and PVC for Jenkins
-  You can check the files [Jenkins-sc.yaml](https://github.com/davabdallah/Atos-Task/blob/main/02.%20Install%20Jenkins/01.%20Jenkins.-SC.yaml), [Jenkins-PV.yaml](https://github.com/davabdallah/Atos-Task/blob/main/02.%20Install%20Jenkins/02.%20Jenkins-PV.yaml), [Jenkins-PVC.yaml](https://github.com/davabdallah/Atos-Task/blob/main/02.%20Install%20Jenkins/03.%20Jenkins-PVC.yaml)  in the same directory

### 3. Create values.yaml and jenkins-casc.yaml
- You can check [Jenkins-values.yaml](https://github.com/davabdallah/Atos-Task/blob/main/02.%20Install%20Jenkins/04.%20Jenkins-values.yaml) files in the same directory
### 4. Install Jenkins using helm
```console 
helm repo add jenkins https://charts.jenkins.io
helm repo update
helm install jenkins jenkins/jenkins -f jenkins-values.yaml --namespace jenkins
```
### 5. Expose the Jenkins service to access
```console
kubectl port-forward service/jenkins 8080:8080 --namespace jenkins
```
### 6. Configure Ingress to be accessible outside
- You can check [jenkins-ingress.yaml](https://github.com/davabdallah/Atos-Task/blob/main/02.%20Install%20Jenkins/06.%20Jenkins-ingress.yaml) file in the same directory
- Add in the hostsfile the workernode public IP and the hostname as configured in ingress file
```console
curl -kv -H "Host: jenkins.atostask.com" http://"workernodeprivateip		
```
### 7. Retrieve the initial admin password
```console
kubectl exec --namespace jenkins -it svc/jenkins -c jenkins -- /bin/cat /run/secrets/additional/chart-admin-password && echo
```
#### Troubleshooting Tips:

- Make Sure All Instances in the same VPC and can connect to each other
  
- Make sure that you give the correct permission to the config file 
```console
 sudo chmod 644 /var/lib/kubelet/config.yaml
```
- If you face any errors you get the logs and the description of the failed pods
```console
kubectl logs jenkins-0 -c init -n Jenkins
```
- and
```console
kubectl decribe pods jenkins-0
```
