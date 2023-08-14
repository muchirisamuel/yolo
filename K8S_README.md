# IP 4 K8S EXPLANATION.

    This is the explanation for the IP 4 project for Kubernetes deployment to GKE.

## MANIFESTS

    I have divided my manifests in three su directories, that is client, ingress, server.

### 1. Client Manifests

    The client manifest containes client-deployment and client services.

#### a) Client Deployment.

    In the client-deployment, the kind is 'Deployment' and the which belongs to 'yolo' namespace.
    It has one replica and the docker image used is from 'bixoloo/client:v1.0.2'.
    The exposed port is 3000.

#### b) Client Service.

    The client service is a kind of 'Service' and has a type of 'LoadBalancer'
    The targetPort is set to 3000.
    This is the port which will be pointed to the url with the ip address to be able to access the application.

### 2. Server Manifests

    The server manifest contains the configurations for the backend application. It has deployment and services manifests.

#### a) Server Deployment.

    In the client-deployment, the kind is 'Deployment' and the which belongs to 'yolo' namespace.
    It has one replica and the docker image used is from 'bixoloo/client:v1.0.2'.
    The exposed port is 3000.

#### b) Server Service.

    The client service is a kind of 'Service' and has a type of 'LoadBalancer'
    The targetPort is set to 5000.
    This is the port which will be used by the nginx to direct traffic to the client.

### 3. Db Manifests

    The db manifest contains configurations for mongodb Persistent Volume Claim, Deployment and Service manifests.

#### a) Persistent Volume Claim Manifest

    The dbv-pvc.yml contains the persistent volume claims instructions so that even if the our pods get restarted or deleted the data stored in the db will
    not be lost.
    Storage is set to 1Gi and accessMode is 'ReadWriteOnce'

#### b) Deployment Manifest

    I am using mongo image for the deployment.
    I have stored db password in environment variable and container port set to 27017.
    The mount path for persistent data to avoid losing data on restart is set to '/data/db'
    As in all other manifests, the namespace is 'yolo' to isolate my deployments from other deployments.
    I have one replica for my mongodb deployment.

#### c) Service Manifest

    The mongodb-service.yml will help us in access the application outside the cluster.
    The nodePort is set to 27017.

### 4. Commands

    I have created a namespace for this project on gke cluster called 'yolo'
    The command to create the namespace is kubectl create namespace yolo.

    To create the deployments and services, I have run this command

#### a) client => kubectl apply -f client

    This created client deployments and services.

#### b) db => kubectl apply -f db

        This created db deployments and services.

#### c) server => kubectl apply -f server

        This created server deployments and services

#### d) ingress => kubectl apply -f ingress

    This created the deployments for nginx load balancing.

