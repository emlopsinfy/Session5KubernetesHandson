In this repo, we will see Kubernetes Handson in local laptop.

Refer Session 4 for working on online terminal provided by K8s - https://github.com/emlopsinfy/Session4KubernetesIntro

The above repo also has detailed overview on Kubernetes.

##### Docker Desktop

Assuming, we have installed Docker Desktop, if not refer this repo - https://github.com/emlopsinfy/docker-install

Refer previous session repos for Docker Handson.

##### Let’s start Kubernetes on Local:

Install Kubectl from https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

Install Minikube from https://minikube.sigs.k8s.io/docs/start/

Minikube : It helps to virtualize our laptop to run different master nodes and worker nodes.

Now open CMD and enter 

##### minikube start --vm-driver=docker

Sample Output :

* minikube v1.23.2 on Microsoft Windows 10 Home Single Language 10.0.19042 Build 19042
* Using the docker driver based on existing profile
* Starting control plane node minikube in cluster minikube
* Pulling base image ...
* Restarting existing docker container for "minikube" ...
* Preparing Kubernetes v1.22.2 on Docker 20.10.8 ...
* Verifying Kubernetes components...
  ! Executing "docker container inspect minikube --format={{.State.Status}}" took an unusually long time: 10.7854085s
* Restarting the docker service may improve performance.
  - Using image gcr.io/k8s-minikube/storage-provisioner:v5
* Enabled addons: default-storageclass, storage-provisioner
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

##### Yes, It is working.

If you get error, follow below steps.

See path in Env Variables, make sure C:\Program Files\Kubernetes\Minikube is above C:\Program Files\Docker\Docker\resources\bin and C:\ProgramData\DockerDesktop\version-bin

This is because we are asking to kubectl from Kubernetes Minikube not from Docker.

If still error persists, 
https://stackoverflow.com/questions/47603689/minikube-not-starting-on-windows-10
PS C:\WINDOWS\system32> minikube delete 
 PS C:\WINDOWS\system32> kubectl config use-context minikube
 PS C:\WINDOWS\system32> minikube start --vm-driver=docker

Delete Docker folder from C:\Users\Admin\AppData\Local and C:\Users\Admin\AppData\Roaming

and try to reinstall Docker, Kubernetes ..



##### Below are the commands

1.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get nodes
NAME       STATUS   ROLES                  AGE   VERSION
minikube   Ready    control-plane,master   8h    v1.22.2

2.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl version
Client Version: version.Info{Major:"1", Minor:"21", GitVersion:"v1.21.5", GitCommit:"aea7bbadd2fc0cd689de94a54e5b7b758869d691", GitTreeState:"clean", BuildDate:"2021-09-15T21:10:45Z", GoVersion:"go1.16.8", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.2", GitCommit:"8b5a19147530eaac9476b0ab82980b4088bbc1b2", GitTreeState:"clean", BuildDate:"2021-09-15T21:32:41Z", GoVersion:"go1.16.8", Compiler:"gc", Platform:"linux/amd64"}

3.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>minikube status
! Executing "docker container inspect minikube --format={{.State.Status}}" took an unusually long time: 13.84889s

* Restarting the docker service may improve performance.
  minikube
  type: Control Plane
  host: Running
  kubelet: Running
  apiserver: Running
  kubeconfig: Configure

4.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pod
No resources found in default namespace.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get deployments
No resources found in default namespace.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get replicasets
No resources found in default namespace.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   8h

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get configmap
NAME               DATA   AGE
kube-root-ca.crt   1      8h

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get secrets
NAME                  TYPE                                  DATA   AGE
default-token-ttft2   kubernetes.io/service-account-token   3      8h

Currently No Pods, Deployment, replicasets and default service,secrets,cofigmap from Kubernetes.



5.

create deployment

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl create deployment nginx-depl --image=nginx
deployment.apps/nginx-depl created

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx-depl   1/1     1            1           3m56s

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-5c8bf76b5b-hzhhg   1/1     Running   0          2m28s

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get replicasets
NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-5c8bf76b5b   1         1         1       4m50s

Deployment created and pods started running, replica created.

##### Deployment manages the replicasets, replicasets manages all the pods, pods are abstraction on top of container.

6.

Edit Deployment

##### kubectl edit deployment nginx-depl

It opens an yaml file, changed image from nginx to nginx:1.16, saved, closed.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl edit deployment nginx-depl
deployment.apps/nginx-depl edited

again same command - kubectl edit deployment nginx-depl

it opened, edited replicas to 2, saved, closed.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-7fc44fc5d4-cch8f   1/1     Running   0          39s
nginx-depl-7fc44fc5d4-fhl5k   1/1     Running   0          86s

Now two pods created!

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get replicasets
NAME                    DESIRED   CURRENT   READY   AGE
nginx-depl-5c8bf76b5b   0         0         0       43m
nginx-depl-7fc44fc5d4   2         2         2       2m24s

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
nginx-depl   2/2     2            2           43m

Two pods are ready in deployment.

Edited again image to nginx.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl edit deployment nginx-depl
deployment.apps/nginx-depl edited

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods
NAME                          READY   STATUS        RESTARTS   AGE
nginx-depl-5c8bf76b5b-kkqdt   1/1     Running       0          10s
nginx-depl-5c8bf76b5b-vhd6z   1/1     Running       0          19s
nginx-depl-7fc44fc5d4-cch8f   1/1     Terminating   0          3m23s

See, now its creating new container and terminating old one..

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
nginx-depl-5c8bf76b5b-kkqdt   1/1     Running   0          54s
nginx-depl-5c8bf76b5b-vhd6z   1/1     Running   0          63s

7.

create mongo DB deployment

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl create deployment mongo-depl --image=mongo
deployment.apps/mongo-depl created

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
mongo-depl-5fd6b7d4b4-zxvzd   1/1     Running   0          28s
nginx-depl-5c8bf76b5b-kkqdt   1/1     Running   0          3m12s
nginx-depl-5c8bf76b5b-vhd6z   1/1     Running   0          3m21s

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get replicasets
NAME                    DESIRED   CURRENT   READY   AGE
mongo-depl-5fd6b7d4b4   1         1         1       52s
nginx-depl-5c8bf76b5b   2         2         2       48m
nginx-depl-7fc44fc5d4   0         0         0       7m36s

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods -o wide
NAME                          READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
mongo-depl-5fd6b7d4b4-zxvzd   1/1     Running   0          2m29s   172.17.0.2   minikube   <none>           <none>
nginx-depl-5c8bf76b5b-kkqdt   1/1     Running   0          5m13s   172.17.0.4   minikube   <none>           <none>
nginx-depl-5c8bf76b5b-vhd6z   1/1     Running   0          5m22s   172.17.0.5   minikube   <none>           <none>



8.

Interact with container

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl exec -it mongo-depl-5fd6b7d4b4-zxvzd -- bin/bash
root@mongo-depl-5fd6b7d4b4-zxvzd:/# ls
bin  boot  data  dev  docker-entrypoint-initdb.d  etc  home  js-yaml.js  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@mongo-depl-5fd6b7d4b4-zxvzd:/# exit
exit

9.

deleting

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl delete deployment mongo-depl
deployment.apps "mongo-depl" deleted

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl delete deployment nginx-depl
deployment.apps "nginx-depl" deleted

10.

create yaml file

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>NUL > nginx-deployment.yaml
Access is denied.

11.

##### Create deployment using yaml file

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment created

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   2/2     2            2           7m22s

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-7848d4b86f-lv9qm   1/1     Running   0          8m7s
nginx-deployment-7848d4b86f-vckfx   1/1     Running   0          8m7s

Open, edit the yaml file again, then apply to get changes reflected.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl apply -f nginx-deployment.yaml
deployment.apps/nginx-deployment configured

It creates new pods for you..

##### YAML file

metadata name is deployment name

selector name will be used for communication

##### deployment yaml file:

##### Container port is app specific port like phone number

#### service yaml file

##### port is the service port, its for connecting service.

##### target port is the same container port which we mentioned in deployment file, its for connecting to port from service.

12.

##### look config file which kubernetes made

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl edit deployment nginx-deployment

It opens config file.

13.

##### create service using yaml file

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl apply -f nginx-service.yaml
service/nginx-service created

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get services
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP   12h
nginx-service   ClusterIP   10.97.16.254   <none>        80/TCP    89s

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get pods -o wide
NAME                              READY   STATUS    RESTARTS   AGE     IP           NODE       NOMINATED NODE   READINESS GATES
nginx-deployment-d6dcb985-ns4gv   1/1     Running   0          7m17s   172.17.0.4   minikube   <none>           <none>
nginx-deployment-d6dcb985-rwtzb   1/1     Running   0          7m27s   172.17.0.2   minikube   <none>           <none>

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl describe service nginx-service
Name:              nginx-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=nginx
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.97.16.254
IPs:               10.97.16.254
Port:              <unset>  80/TCP
TargetPort:        8080/TCP
Endpoints:         172.17.0.2:8080,172.17.0.4:8080
Session Affinity:  None
Events:            <none>



##### see IPs of the pods are same as endpoints while we described.

172.17.0.2:8080,172.17.0.4:8080



14.

To take nginx config file and save it locally.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson>kubectl get deployment nginx-deployment -o yaml > nginx-deployment-result.yaml

##### Deleted deployments, services.



##### Assignment

We are going to create the MongoDB Pod.

Since MongoDB Pod IPs keep on changing, we are going to attach it to the service.

This MongoDB Service is going to talk to another Pod (Mongo-express container), this Pod also attached to a Service.

This Service will be exposed to public URL.

If we are do this in cloud, we will get access to this public URL.

Since we are doing it in Minikube docker system, we will be having tunnel to access that.



For the second pod to talk to first pod (MongoDB pod), it needs credentials and that can be taken from secrets.

It also needs first pod name (basically DB URL) and that can be taken from config map.

Let’s start..

#### Refer files inside mongo folder, comments are added.

##### First create mongo-deployment.yaml file

apiVersion: apps/v1

kind: Deployment

metadata:

  name: mongodb-deployment

  labels:

​    app: mongodb #this is to talk to this deployment, this is label for deployment, this can be anything.

spec:

  selector:

​    matchLabels:

​      app: mongodb #if we match this, we will be able to reach this, this name and below label name must be same

  replicas: 1

  template:

​    metadata:

​      labels:

​        app: mongodb #this must match with above name

​    spec:

​      containers:

​      \- name: mongodb

​        image: mongo

​        ports:

​        \- containerPort: 27017

​          \#Connect to MongoDB from another Docker container

​          \#The MongoDB server in the image listens on the standard MongoDB port, 27017, 

​          \#so connecting via Docker networks will be the same as connecting to a remote mongod. 

​          \#The following example starts another MongoDB container instance and runs the 

​          \#mongo command line client against the original MongoDB container from the example above, 

​          \#allowing you to execute MongoDB statements against your database instance:        

​        env:

​        \- name: MONGO_INITDB_ROOT_USERNAME

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-username

​        \- name: MONGO_INITDB_ROOT_PASSWORD

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-password

##### Then create mongo-secret.yaml file

apiVersion: v1

kind: Secret

metadata:

  name: mongodb-secret

type: Opaque

data:

  mongo-root-username: dXNlcm5hbWU=

  mongo-root-password: cGFzc3dvcmQ=

##### Now, apply secret first, then deployment

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl apply -f mongo-secret.yaml
secret/mongodb-secret unchanged

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl apply -f mongo-deployment.yaml
deployment.apps/mongodb-deployment unchanged

##### check deployments

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl get deployments
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
mongodb-deployment   1/1     1            1           3m35s

##### create service

We are going to create service inside same deployment yaml file, since it makes sense to keep deployment and service together in same yaml file.

Added service in same yaml file

apiVersion: apps/v1

kind: Deployment

metadata:

  name: mongodb-deployment

  labels:

​    app: mongodb #this is to talk to this deployment, this is label for deployment, this can be anything.

spec:

  selector:

​    matchLabels:

​      app: mongodb #if we match this, we will be able to reach this, this name and below label name must be same

  replicas: 1

  template:

​    metadata:

​      labels:

​        app: mongodb #this must match with above name

​    spec:

​      containers:

​      \- name: mongodb

​        image: mongo

​        ports:

​        \- containerPort: 27017

​          \#Connect to MongoDB from another Docker container

​          \#The MongoDB server in the image listens on the standard MongoDB port, 27017, 

​          \#so connecting via Docker networks will be the same as connecting to a remote mongod. 

​          \#The following example starts another MongoDB container instance and runs the 

​          \#mongo command line client against the original MongoDB container from the example above, 

​          \#allowing you to execute MongoDB statements against your database instance:        

​        env:

​        \- name: MONGO_INITDB_ROOT_USERNAME

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-username

​        \- name: MONGO_INITDB_ROOT_PASSWORD

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-password   

\---

apiVersion: v1

kind: Service

metadata:

  name: mongodb-service

spec:

  selector:

​    app: mongodb

  ports:

  \- protocol: TCP

​    port: 27017 #this is where service listens, service port

​    targetPort: 27017 #this is your db specfic port, it is taken from hub.

​    \#so we are listening and redirecting to same port, listening can be other port too.           

##### Now apply again..

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl apply -f mongo-deployment.yaml
deployment.apps/mongodb-deployment unchanged
service/mongodb-service created

##### service created, deployment unchanged.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl get all
NAME                                     READY   STATUS    RESTARTS   AGE
pod/mongodb-deployment-8f6675bc5-289df   1/1     Running   0          26m

NAME                      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)     AGE
service/kubernetes        ClusterIP   10.96.0.1      <none>        443/TCP     3d6h
service/mongodb-service   ClusterIP   10.108.111.8   <none>        27017/TCP   19m

NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongodb-deployment   1/1     1            1           26m

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/mongodb-deployment-8f6675bc5   1         1         1       26m

##### checking pods IP

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl get pods -o wide
NAME                                 READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
mongodb-deployment-8f6675bc5-289df   1/1     Running   0          28m   172.17.0.2   minikube   <none>           <none>

##### It is 172.17.0.2

##### Now checking ClusterIP from Service

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl get services
NAME              TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)     AGE
kubernetes        ClusterIP   10.96.0.1      <none>        443/TCP     3d6h
mongodb-service   ClusterIP   10.108.111.8   <none>        27017/TCP   20m

##### It is 10.108.111.8

##### Now describing service to see above two IPs matches.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl describe service mongodb-service
Name:              mongodb-service
Namespace:         default
Labels:            <none>
Annotations:       <none>
Selector:          app=mongodb
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.108.111.8
IPs:               10.108.111.8
Port:              <unset>  27017/TCP
TargetPort:        27017/TCP
Endpoints:         172.17.0.2:27017
Session Affinity:  None
Events:            <none>

##### It is matching, Pod IP are there in Endpoints, ClusterIP are there in IP.

##### create second pod - mongo-express

apiVersion: apps/v1

kind: Deployment

metadata:

  name: mongodb-express

  labels:

​    app: mongodb-express #this is to talk to this deployment, this is label for deployment, this can be anything.

spec:

  selector:

​    matchLabels:

​      app: mongodb-express #if we match this, we will be able to reach this, this name and below label name must be same

  replicas: 1

  template:

​    metadata:

​      labels:

​        app: mongodb-express #this must match with above name

​    spec:

​      containers:

​      \- name: mongo-express

​        image: mongo-express

​        ports:

​        \- containerPort: 8081

​        env:

​        \- name: ME_CONFIG_MONGODB_ADMINUSERNAME

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-username

​        \- name: ME_CONFIG_MONGODB_ADMINPASSWORD

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-password

​        \- name: ME_CONFIG_MONGODB_SERVER

​          valueFrom:

​            configMapKeyRef:

​              name: mongodb-configmap

​              key: database_url

It is taking credentials from mongo-secret and its talking to first service through database url through config map.

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl apply -f mongo-configmap.yaml
configmap/mongodb-configmap created

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl apply -f mongo-express.yaml
deployment.apps/mongodb-express created

##### Add a service for mongodb-express

apiVersion: apps/v1

kind: Deployment

metadata:

  name: mongodb-express

  labels:

​    app: mongodb-express #this is to talk to this deployment, this is label for deployment, this can be anything.

spec:

  selector:

​    matchLabels:

​      app: mongodb-express #if we match this, we will be able to reach this, this name and below label name must be same

  replicas: 1

  template:

​    metadata:

​      labels:

​        app: mongodb-express #this must match with above name

​    spec:

​      containers:

​      \- name: mongo-express

​        image: mongo-express

​        ports:

​        \- containerPort: 8081

​        env:

​        \- name: ME_CONFIG_MONGODB_ADMINUSERNAME

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-username

​        \- name: ME_CONFIG_MONGODB_ADMINPASSWORD

​          valueFrom:

​            secretKeyRef:

​              name: mongodb-secret

​              key: mongo-root-password

​        \- name: ME_CONFIG_MONGODB_SERVER

​          valueFrom:

​            configMapKeyRef:

​              name: mongodb-configmap

​              key: database_url

\---

apiVersion: v1

kind: Service

metadata:

  name: mongodb-express-service

spec:

  selector:

​    app: mongodb-express

  type: LoadBalancer

  ports:

  \- protocol: TCP

​    port: 8081 #this is where service listens, service port

​    targetPort: 8081 #this is your db specfic port, it is taken from hub.

​    \#so we are listening and redirecting to same port, listening can be other port too.

​    nodePort: 30000 #we are exposing 

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl apply -f mongo-express.yaml
deployment.apps/mongodb-express unchanged
service/mongodb-express-service created



##### check services

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>kubectl get services
NAME                      TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
kubernetes                ClusterIP      10.96.0.1       <none>        443/TCP          3d9h
mongodb-express-service   LoadBalancer   10.98.237.183   <pending>     8081:30000/TCP   30s
mongodb-service           ClusterIP      10.108.111.8    <none>        27017/TCP        151m

See external IP pending for mongodb-express-service

since we are using minikube on Virtualized system on Virtualized node, its difficult to get IP.

But AWS provides that!

So...

We are going to use tunnel to talk to that service.



C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>minikube service mongodb-express-service
! Executing "docker container inspect minikube --format={{.State.Status}}" took an unusually long time: 2.3089218s

* Restarting the docker service may improve performance.
  |-----------|-------------------------|-------------|---------------------------|
| NAMESPACE   | NAME                      | TARGET PORT   | URL                         |
| ----------- | ------------------------- | ------------- | --------------------------- |
| default     | mongodb-express-service   | 8081          | http://192.168.49.2:30000   |
| ----------- | ------------------------- | ------------- | --------------------------- |
* Starting tunnel for service mongodb-express-service.
  |-----------|-------------------------|-------------|------------------------|
| NAMESPACE   | NAME                      | TARGET PORT   | URL                      |
| ----------- | ------------------------- | ------------- | ------------------------ |
| default     | mongodb-express-service   |               | http://127.0.0.1:52540   |
| ----------- | ------------------------- | ------------- | ------------------------ |
* Opening service default/mongodb-express-service in default browser...
  ! Because you are using a Docker driver on windows, the terminal needs to be open to run it.

http://127.0.0.1:52540 -- this is available to us

##### MongoDB talking to MongoDB-service, MongoDB-Service talking to Mongo-express container,

##### mongo-express container is talking to service, that service is exposed with that ip address.

##### For the Dashboard

C:\Users\Admin\Desktop\Raajesh\EMLO\Session5KubernetesHandson\mongo>minikube dashboard --url
! Executing "docker container inspect minikube --format={{.State.Status}}" took an unusually long time: 3.1707735s
* Restarting the docker service may improve performance.
* Enabling dashboard ...
  - Using image kubernetesui/metrics-scraper:v1.0.7
  - Using image kubernetesui/dashboard:v2.3.1
* Verifying dashboard health ...
* Launching proxy ...
* Verifying proxy health ...
  http://127.0.0.1:52580/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/




































