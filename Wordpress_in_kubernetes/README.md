
# Wordpress_in_Kubernetes
Kubernetes full edge wordpress configuration using pvc, pv, secrets, configmap and load balancer.

## Setup

**Clone the repository**
>git clone < Repository_URL >

## 1. Prerequisite
- ### Minukube

**Download and Install Minikube**

>curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
>
Add the Minikube binary to the executable path:
>sudo install minikube-linux-amd64 /usr/local/bin/minikube

Verify Installation
>minikube version

## 2. Apply Wordpress Manifests

### 2.1 ConfigMap for mysql
ConfigMap is used for storing non-confidential data like environment variables that can be passed to deployment manifests.
>vim configmap.yaml
>
  
  **Apply**
>kubectl apply -f configmap.yaml
>
 ### 2.2 Secret Key for Wordpress

Secret keys are used to store credentials that are need to be confidential. I It store data in encrypted form. 
 
**Apply**
>kubectl apply -f secret.yaml

### 2.3 Set up PVC for MySQL
It is used to Create Volumes for persistent data storage. So MySQL database will stay safe even when any container crashes.

**Apply**
>kubectl apply -f mysql-pvc.yaml

### 2.4 Set up PVC for Wordpress

For Preserving application data, pvc is sued with wordpress. This volume can be set up to share with number of replicas of wordpress or any other application.
>
**Apply**
>kubectl apply -f wordpress-pvc.yaml


## 3. Apply Deployment Files

### 3.1 Mysql Deploy
        
**Apply**

>kubectl apply -f mysql-deployment.yaml
>

### 3.2 Wordpress Deploy

**Apply**
>kubectl apply -f wordpress-deployment.yaml


 **4. Get the minikube ip with the following command.**
 >minikube ip
 >
 and browse the cluster ip of wordpress service and browse it in this pattern.
  >minikube_ip:wordpress_service_port
 >
**example**
>192.168.52.8:31370


# Conclusion

Deploying WordPress in a Kubernetes environment with PVCs, ConfigMaps, and Secrets provides a robust and flexible solution for hosting a WordPress site. This setup leverages Kubernetes' capabilities to enhance scalability, security, and management, ensuring a smooth and efficient deployment process.
