<h1 align="center">k8s-Helm</a> 
<img src="https://github.com/blackcater/blackcater/raw/main/images/Hi.gif" height="32"/></h1>


### <a name="road">ROADMAP</a>
  - #### [Info](#info)
  - #### [Install Helm](#helm)
  - #### [Deploy Nginx via helm with Ingress configuration](#nginx)
  - #### [Deploy Pacan-game via helm with Ingress configuration](#pacman)
  - #### [Deploy MERN stack via helm](#mern)

---

### <a name="info">SOME INFO</a> | [ROADMAP](#road)

In this project i've:
  - created HelmChart for deploying Nginx replica set via Helm
  - created HelmChart for deploying Pacman game via Helm
  - installed MERN stack via Helm with plus ingress for routing 
    
So lets go through all the stages.

---

### <a name="helm">INSTALL HELM</a> | [ROADMAP](#road)

There are several ways to install Helm. I prefer using apt for Debian based distros:

```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```
>Check other ways by link [here](https://helm.sh/docs/intro/install/)

---

### <a name="nginx">NGINX HELM</a> | [ROADMAP](#road)

The main idea of using Helm is simplicity while deploying objects to k8s.

For deploying Nginx i've prepared code [here](https://github.com/digitalake/k8s-Helm/tree/main/nginx):
  - Chart.yaml  --> info about the chart
  - values.yaml --> values file for insertion while templating
  - templates/  --> directory with manifest templates:
    - ingress.yaml
    - service.yaml
    - nginx.yaml
    - _helpers.yaml(with the function to use while templating)
    
Installing chart:
```
helm install <release-name> <path>
```
![Знімок екрана_20230212_174108](https://user-images.githubusercontent.com/109740456/218328582-e106a91d-317d-4de6-b3a8-da37581e4fbf.png)

Checking the created release:
```
helm ls
```
![Знімок екрана_20230212_174121](https://user-images.githubusercontent.com/109740456/218328640-b783bf91-df0e-42af-bc3b-a89c7908ee74.png)

Checking deployed pods:
```
kubectl get pods
```
<img src="https://user-images.githubusercontent.com/109740456/218328668-4dab64e9-93a5-48a4-a466-43b45ad329e4.png" width="550">

Checking if the Nginx is avaliable by public ip by typing URL:
```
https://<domain-name>/prefix
```
>Access my nginx by link [here](https://vantus.dns.army/mynginx/)

Result:

<img src="https://user-images.githubusercontent.com/109740456/218328783-b95bd402-d659-42a4-8040-cfbf1199e566.png" width="550">

Creating customValues.yaml file:
```
vim customValues.yaml
```
Adding some value (code sample):
```
replicaCount: 4
```
Check the file:
```
less customValues.yaml
```
<img src="https://user-images.githubusercontent.com/109740456/218329115-3dfb86a9-75e3-4c79-a21f-f992ac62d5d0.png" width="200">

Upgrading with helm:
```
helm upgrade -f <custom-file> <release-name> <path>
```
![Знімок екрана_20230212_174611](https://user-images.githubusercontent.com/109740456/218329074-5bb8a255-2f0a-4359-83c5-b3f5857f4edf.png)

Check the changes were done to replication counter:
```
kubectl get pods
```
<img src="https://user-images.githubusercontent.com/109740456/218329168-e7d520d8-7274-4db9-a5dc-e868599f5c31.png" width="550">

---

### <a name="pacman">PACMAN HELM</a> | [ROADMAP](#road)

For deploying Pacman i've prepared code [here](https://github.com/digitalake/k8s-Helm/tree/main/pacman):
  - Chart.yaml  --> info about the chart
  - values.yaml --> values file for insertion while templating
  - templates/  --> directory with manifest templates:
    - ingress.yaml
    - service.yaml
    - pacman.yaml
    - _helpers.yaml(with the function to use while templating)

Installing chart:
```
helm install <release-name> <path>
```
![Знімок екрана_20230212_180429](https://user-images.githubusercontent.com/109740456/218329183-e8c81934-de75-4583-b674-8226e9693716.png)

Checking deployed pods:
```
kubectl get pods
```
<img src="https://user-images.githubusercontent.com/109740456/218329214-1cba9207-d3be-4672-998c-110ff6482709.png" width="550">

Checking if the Pacman is avaliable by public ip by typing URL:
```
https://<domain-name>
```
>Access my pacman by link [here](https://vantus.dns.army/)

Result:

<img src="https://user-images.githubusercontent.com/109740456/218329268-0a31489f-bdbb-4453-b98d-1c1328b58169.png" width="550">

---

### <a name="mern">MERN HELM</a> | [ROADMAP](#road)

For deploying MERN i've prepared code [here](https://github.com/digitalake/k8s-Helm/tree/main/mern-ingress):
  - mern-ingress.yaml  --> k8s manifest about to make MERN avaliable with public ip
  
Add bitnami repo and install the MERN chart:
```
helm repo add bitnami https://charts.bitnami.com/bitnami
```
```
helm install mern-stack bitnami/node
```
![Знімок екрана_20230212_193059](https://user-images.githubusercontent.com/109740456/218329423-4e3c59bf-cf5d-4f4a-90e2-87e643b76b10.png)


Install the ingress via kubectl:
```
kubectl apply -f <manifest>
```
<img src="https://user-images.githubusercontent.com/109740456/218329452-b13587fc-9529-45ce-927e-8ddd7a09fdd9.png" width="500">

Checking deployed pods:
```
kubectl get pods
```
<img src="https://user-images.githubusercontent.com/109740456/218329499-d27b9591-0300-44f7-a4d9-cddc1c3f12fa.png" width="550">

Checking if the MERN is avaliable by public ip by typing URL:
```
https://<domain-name>/prefix
```
>Access my MERN by link [here](https://vantus.dns.army/mern)

Result:

<img src="https://user-images.githubusercontent.com/109740456/218329574-0996d86a-acd5-4315-9fb1-95876f7bfa33.png" width="550">






