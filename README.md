# webAppMongoDB


- Web Application on k82 cluster with Mongo DB 

- Deploy for MongoDB database
    * service

- Deplloyment for Web Application 
    * NGINX

- External configuration for DB URL and Credentials
    * ConfigMap 
    * Secret

- External service for external access



# K8s Components Overview

* Creating 4 config files.
    - ConfigMap               --- MongoDB Endpoint 
        data: database-url (the service that gets the requests from the webApp)

    - Sercret                 --- MongoDB User & Password
        type: Opqure (default)
        data: 
        mongodb-user: bash -c 'echo -n mongouser | base64'
        mongodb-pass: bash -c 'echo -n mongopassword | base64'

    - dbDeployment-&-service  --- MongoDB Apllication with internal service
        kind: 
            - deployment (create a cluster of the db container)
            - service (create a service for the database exposed by the requests from the wabApp)
        environment:
            - name: SECRETS_FILE_FORM_SERVICE_IN_K8S
                valueFrom:
                secretKeyRef:
                    name: k8s-secret
                    key: key-name
    
    - webAppDeployment-&-service --- web Apllication with external service
        kind:
            - deployment (create a cluster of the webapp container)
            - service (create a service for the web app exposed  the wabApp)
                type: NodePort (for exposing to outside world)
        environment:
            - name: SECRETS_FILE_FORM_THE_CONTAINER
                valueFrom:
                secretKeyRef:
                    name: k8s-secret
                    key: key-name



# Creating the Components with kubectl commands 
 ## Create the configuration setting for the cluster
    $ kubectl apply -f mongo-config.yaml
##  Create the secret setting for the cluster
    $ kubectl apply -f mongo-secret.yaml
##  Create the dbDeployment setting for the cluster
    $ kubectl apply -f mongo-deployment.yaml
##  Create the webAppDeployment setting for the cluster
    $ kubectl apply -f webapp-deployment.yaml


### Check the Components - 
    $ kubectl get all [get the all cluster]
    $ kubectl get configMap [list the config map]
    $ kubectl get secret [list the secret serivce]    
    $ kubectl het svc [list the routing table of the cluster]
    $ kubectl describe service <serviceName>
    $ minikube ip [show the ip of the minikube runner]
    $ kubectl get node -o wide [show wide display]
    $ kubectl get svc -o wide 