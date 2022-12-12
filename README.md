# Azure-DevOps-BoschAI-Techtrends
Python Flask web application based on a microservice.  Docker, GithubAction ,Kubernetes,ArgoCD(CI/CD)

## Techtrends project 

Step 1: Creating the docker images and taging it: Docker for Application Packaging 
### Docker contents here 

## Docker Login

docker login

## Commands used to build the application

docker build -t techtrends .

## commands to test and run the application 

docker run -dp 7111:3111 techtrends

## Docker run output 

f125c84d1bc0c6805cfe4d6fdb50999873ebd4faaf0f4f580ac46dcd6e5047de

## Docker command to retrieve the logs 

docker logs <container ID>

## TechTrends logs

Put the output from the logs 

## Docker command to tag the image 

docker tag techtrends yourrepo/techtrends:v1.0.0 

## Docker command to push the images to dockerhub 

docker push yourrepo/techtrends:v1.0.0

docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname



Step 2: Github actions: Continuous Integration with Github Actions
        1. Create a new repo
        2. push your codes to the new repo
        3. Add the docker token and GitHub encrypted secrets from the project directory Goto `settings` > `secret` > `Actions` > click `New repository secret`
        4. create the `techtrends-dockerhub.yml` in the `.github/workflows/` Might be created automatically when creating the github action.
        5. Goto `Github Actions` and click on the `create a new workflow yourself` button

![Docker Secret](screenshots/docekr_secrets.png "Docker Secret")


Step 3: Kubernetetes Declarative Manifests 

## Deploy a Kubernetes cluster

Make sure your oracle VM Box is open

# create a vagrant box using the Vagrantfile in the current directory
`vagrant up`

![VM box](screenshots/Vagrantup.png "VM Box")

# SSH into the vagrant box
# Note: this command uses the .vagrant folder to identify the details of the vagrant box, you can ls to make sure it is included
`vagrant ssh`

# Deploy the Kubernetes cluster from the k3s documentation 

`curl -sfL https://get.k3s.io | sh - `

# Give yourself the root access to kubeconfig 

`sudo su`

## Get all nodes 

`kubectl get no`

## create your Kubernetes Declarative Manifests file namespace.yaml, deploy.yaml and service.yaml

1. make a new file called namespace.yaml and vim into it to add your files 
2. run below codes 

```
touch namespace.yaml
touch deploy.yaml
vim deploy.yaml
touch service.yaml
vim service.yaml
```

```

kubectl apply -f namespace.yaml
kubectl apply -f deploy.yaml
kubectl apply -f service.yaml

```
kubectl delete -f namespace.yaml

Get all Kubectl namespace 

`` kubectl get all -n sandbox ``

Gett all running pods 
`` kubectl get po -A ``

Step 4: Helm Charts

1. create the folder templates/ and add all requires files.
2. create the chart.yaml, values.yaml etc 

Step 5: Continuous Delivery with ArgoCD

1. install AgroCD in your VM Box

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
Note: AFter running the command, you need to wait for sometime to get the container running after which you can continue

## get all pods

`` kubectl get po -n argocd ``

## get all services

`` kubectl get svc -n argocd ``

Now we need to expose it to the internet
First, you need to get the argocd-server from the list of service 
and copy the yaml ` argocd-server-nodeport.yaml` from here https://github.com/udacity/nd064_course_1/blob/main/solutions/argocd/argocd-server-nodeport.yaml 

touch argocd-server-nodeport.yaml
vim argocd-server-nodeport.yaml
kubectl apply -f argocd-server-nodeport.yaml

`` kubectl get svc -n argocd `` again 

then access the ArgoCD UI by going to https://192.168.50.4:30008 or http://192.168.50.4:30007
To login to argocd 
username: admin
password: run command `` kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo ``
generated password: MPxL7340G95KVVru

kubectl describe po -n sandbox


``touch helm-techtrends-staging.yaml``
``vim helm-techtrends-staging.yaml``

``touch helm-techtrends-prod.yaml``
``vim helm-techtrends-prod.yaml``

``kubectl apply -f helm-techtrends-staging.yaml``
``kubectl apply -f helm-techtrends-prod.yaml``

``
