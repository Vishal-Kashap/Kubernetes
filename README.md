## Wordpress_in_Kubernetes
Kubernetes full edge wordpress configuration using pvc, pv, secrets, configmap and load balancer.

**Clone the repository**
>git clone https://github.com/Vishal-Kashap/Wordpress_in_Kubernetes.git

### Create ConfigMap for mysql
>vim configmap.yaml
>
*Set Credentials according to your need.*
>
>apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-config
data:
  DATABASE: "db1"
  USER: "vishal"
  PASSWORD: "12345678"
  ROOT_PASSWORD: "12345678"
  
  **Apply**
>kubectl apply -f configmap.yaml
  ### Create Secret Key for Wordpress environment variables
>vim secret.yaml
>
**Code**

>apiVersion: v1
kind: Secret
metadata:
  name: wp-secret
type: Opaque
data:
  HOST: "bXlzcWw6MzMwNg=="
  DATABASE: "ZGIx"
  USER: "dmlzaGFs"
  PASSWORD: "MTIzNDU2Nzg="
  ROOT_PASSWORD: "MTIzNDU2Nzg="

**Apply**
>kubectl apply -f secret.yaml

## Create Volumes for persistent data storage
### volume for mysql

>vim mysql-pvc.yaml

**Code**
>apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-mysql
  labels:
    app: test
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
      
**Apply**
>kubectl apply -f mysql-pvc.yaml

### volume for wordpress

>vim wordpress-pvc.yaml

**Code**
>apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-wordpress
  labels:
    app: test
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
      
**Apply**
>kubectl apply -f wordpress-pvc.yaml


## Build Deployment Files

### Mysql Deploy

>vim mysql-deployment.yaml
>
**Code**
>apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: mysql
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: mysql
    spec:
      containers:
        - env:
            - name: MYSQL_DATABASE
              valueFrom:
                configMapKeyRef:
                  name: mysql-config
                  key: DATABASE
            - name: MYSQL_USER
              valueFrom:
                configMapKeyRef:
                  name: mysql-config
                  key: USER
            - name: MYSQL_PASSWORD
              valueFrom:
                configMapKeyRef:
                  name: mysql-config
                  key: PASSWORD
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                configMapKeyRef:
                  name: mysql-config
                  key: ROOT_PASSWORD
          image: mysql:5.7
          name: mysql
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: mysql-persistent-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-persistent-storage
          persistentVolumeClaim:
            claimName: pvc-mysql

status: {}

**Apply**

>kubectl apply -f mysql-deployment.yaml
>

### Wordpress Deploy
>vim wordpress_deployment.yaml

**Code**

>apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: wordpress
  name: wp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: wordpress
    spec:
      containers:
        - env:
          - name: WORDPRESS_DB_HOST
            valueFrom:
              secretKeyRef:
                name: wp-secret
                key: HOST
          - name: WORDPRESS_DB_USER
            valueFrom:
              secretKeyRef:
                name: wp-secret
                key: USER
          - name: WORDPRESS_DB_PASSWORD
            valueFrom:
              secretKeyRef:
                name: wp-secret
                key: PASSWORD
          - name: WORDPRESS_DB_NAME
            valueFrom:
              secretKeyRef:
                name: wp-secret
                key: DATABASE
          image: wordpress:5.2
          name: wordpress
          ports:
          - containerPort: 80
          volumeMounts:
            - name: wordpress-persistent-storage
              mountPath: /var/www/html
      volumes:
        - name: wordpress-persistent-storage
          persistentVolumeClaim:
            claimName: pvc-wordpress

status: {}

**Apply**
>kubectl apply -f wordpress-deployment.yaml


Get the COntainr

 **Get the minikube ip with the following command.**
 >minikube ip
 >
 and browse the cluster ip of wordpress service and browse it in this pattern.
  >minikube_ip:wordpress_service_port
 >
**example**
>192.168.52.8:31370

