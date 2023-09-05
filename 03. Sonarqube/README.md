# The below Steps to Install Jenkins using Helm and Enable Ingress

### 1. Switch to SonarQube namespace
```console
kubectl config set-context --current --namespace=sonarqube
```
### 2. Create sonarqube-cs.yaml , sonarqube-pv.yaml and sonarqube-PVC.yaml for Sonarqube
- You can check the files [sonarqube-sc.yaml](https://github.com/davabdallah/Atos-Task/blob/main/03.%20Sonarqube/01.%20sonarqube-sc.yaml),  [sonarqube-pv.yaml](https://github.com/davabdallah/Atos-Task/blob/main/03.%20Sonarqube/02.%20sonarqube-pv.yaml),  [sonarqube-pvc.yaml](https://github.com/davabdallah/Atos-Task/blob/main/03.%20Sonarqube/03.%20sonarqube.pvc.yaml)

```console
kubectl apply -f sonarqube-sc.yaml
```
```console
kubectl apply -f sonarqube-pv.yaml
```
```console
kubectl apply -f sonarqube-pvc.yaml
```

### 3. Create sonarqube-values.yaml
- You can check [sonarqube-values.yaml](https://github.com/davabdallah/Atos-Task/blob/main/02.%20Install%20Jenkins/04.%20Jenkins-values.yaml) files in the same directory
  
### 4. Install SonarQube Developer Edition using helm

```console
helm repo add sonarqube https://SonarSource.github.io/helm-chart-sonarqube
```
```console
helm repo update
```
```console
helm install sonarqube sonarqube/sonarqube -f sonarqube-values.yaml --namespace sonarqube
```

### 5. Expose the Sonarqube service to access
```console
kubectl port-forward service/sonarqube-sonarqube 9000:9000 --namespace sonarqube
```
	
### 6. Configure Ingress to be accessible externally
- You can check sonarqube-ingress.yaml file in the same directory
- Add in the hosts file the worker node public IP and the hostname
 ```console 
curl -kv -H "Host: sonarqube.atostask.com" http://"workernodeprivateip
```

#### Troubleshooting Tips:

- The default username and Password is admin admin

- Make sure that you give the right permission to the local volume files

	  sudo chmod 777 -R /tmp/sonarqube

