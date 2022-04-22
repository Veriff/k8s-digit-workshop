# Kubernetes Workshop

 - [Intro](#intro)
    - [Requirements for the workshop](#requirements-for-the-workshop)
    - [Setup kubectl](#setup-kubectl)
    - [About cluster](#about-cluster)
 - [Workshop](#workshop)
    - [Checkout workshop files](#checkout-workshop-files)
    - [Run first container](#run-first-container)
    - [Use deployment](#use-deployment)
    - [Expose service](#expose-service)
    - [Ingress](#ingress)

## Intro
### Requirements for the workshop

 - Computer running Mac, Linux or Windows
 - some previous experience of using the terminal
 - Git 
 - code editor

### Setup kubectl

First make sure you have `kubectl` (Kubernetes command-line tool) installed. In order to make it follow instructions in:
  - https://kubernetes.io/docs/tasks/tools/#kubectl

<details><summary>Install instructions: MAC</summary>
<p>

If you have [Brew](https://brew.sh/) present, install using: 
```sh
brew install kubernetes-cli
# test it works
kubectl version --client
```

Manual install:
```sh
# download kubectl (version 1.23)
curl -LO "https://dl.k8s.io/release/v1.23.0/bin/darwin/amd64/kubectl"
# make executable
chmod a+X kubectl
# check that it works
./kubectl version --client
# move to path
sudo mv kubectl /usr/local/bin/kubectl
```

</p>
</details>
<details><summary>Install instructions: Linux</summary>
<p>

Install using package manager: 
```sh
# or use system specific apt-get, yum, ...
snap install kubectl --classic
# test it works
kubectl version --client
```

Manual install:
```sh
# download kubectl (version 1.23)
curl -LO "https://dl.k8s.io/release/v1.23.0/bin/linux/amd64/kubectl"
# make executable
chmod a+X kubectl
# check that it works
./kubectl version --client
# install to path
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

</p>
</details>

<details><summary>Install instructions: Windows</summary>
<p>

If you have [Chocolatey](https://chocolatey.org/) present, install using: 
```sh
choco install kubernetes-cli -y
# test it works
kubectl version --client
```

Manual install:
```sh
# download kubectl (version 1.23)
curl -LO "https://dl.k8s.io/release/v1.23.0/bin/windows/amd64/kubectl.exe"
# move to path
# or uses locally as ./kubectl.exe
 
# test it works
./kubectl.exe version --client
```
 
<hr/>
</details>

Once done add config to use our test Kubernetes cluster into `~/.kube/config` ([download](https://github.com/Veriff/k8s-workshop/raw/urmet/kubectl-config)). 


```sh
# create ~/.kube config directory if not present
mkdir -p ~/.kube
# add kube config with your favorite IDE to ~/.kube/config
# example here uses vim, but use what you like -> nano, code, ...
vim ~/.kube/config
```

<details><summary>Show kubectl config</summary>
<p>

```yaml
apiVersion: v1
kind: Config
current-context: k8s-digit-workshop
preferences: {}
clusters:
# cluster info
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM1ekNDQWMrZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeU1EUXhNekU0TVRZeU5sb1hEVE15TURReE1ERTRNVFl5Tmxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTHA5CmM4QytrNUI2VVQray9vRGt0b0NkbTk2NmRqNmVzSHhySzJiMFhHcTBQWGFtMFFWVlR0dTRuWlRvblJZWkZFZkcKMDVJS1RINmc4ZWIxaW9RcXZ3ZXNvWUVRaG5FaFBBek1uWlJyNnowOEpaWmdZQnpQcFpZeDJaTVlkbnFRYmZQRQo5NTRYVFgxVUt0ckVwWUw5Y2plNTNVVHdMeEpFNWpPRGRhcWNJbVFyMG1CMGs5UzdseEZob2lYaE1RMVUzdENUClA2ajhETzBRV1VVbUJEWkc1SkRqcWQ0ckVJckhscFhxa2RiZ1lCMksxMDQ5U2gzMmsrVjduVHJvbjA5d0FHc3EKKzdWNHQxZjFraW9IWXlWZWVjMG16Y0gvSXRCeUFyZkFEQmQ4ZmhIdlorYlV6SnFXVk95ZW0rRThFb083S2pDZwpESkpqOFpqcEdCQnNqNkZzNzZVQ0F3RUFBYU5DTUVBd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZDSXFWdTQ2dHF3b0ZsN2I3MmNyNW56dHBENEZNQTBHQ1NxR1NJYjMKRFFFQkN3VUFBNElCQVFBNnVmR2xQcTYreEhrVTBTc3NmS3ZsZkQ2UXJuWmJWSzFic1dmRHkxUFBzOWVEMSszcwozNmMwMWZGU0hBVlJDMXBIUmlsYkFudXBScEppRGxHK1dOR3dEckJ3TEd6Z0tZdE81NkFrbFo1UHRib2licW10CjRJY2h2cVN4ZEw1NXhwS3RmZWdqZnhNa2lKaDd1d1NvMGt4cjE0N3NDV2x0ZmM5aGFEVkZKdUJrOVNWU2ZXSlcKemhVZVRwcDNLSGdRamJqS0szV09mS3c3YkNJdFVMemNWalRJeFBObTJ6VmhmQWR1dFhBM3h3ZURvMVZYMDZXawo1Y1lyZkd5WVVwSGZTUFZWbkRNM3p3Kzc0aXNKWmd4aWQ1ME11Z3lDcEliRlRoeURMaG9pb0oyMW9pNVVraXM1CkF0RHhqL3VYTjdhNVZRSGc4NUZUdVNBcTl2dXRZZDRWaEliQwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    server: https://14AE70BA3791C735370E9252575A533A.yl4.eu-north-1.eks.amazonaws.com
  name: k8s-digit-workshop--cluster
users:
# auth info
- name: k8s-digit-workshop--user
  user:
    token: eyJhbGciOiJSUzI1NiIsImtpZCI6InV6dkF1NUN6U0tiLUplTUQtbXl0WHFZRU5INWxrLWdwbEVLLV9rTlAzZUEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJ1cm1ldCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJ1cm1ldC10b2tlbi1mcDg2cCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJ1cm1ldCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImY1MGYyN2MxLTUzYWQtNDQwMC1hN2Q3LTI0NTE4MWRkNTVhOCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDp1cm1ldDp1cm1ldCJ9.iWQ62iSvM9Ln47-6N3WMjeWZBAeS4ak9eVUOON-E8D8flFpvLpRdKjiUv1o-uKj2hPn7f_I58CK4kOPNB4AZyr9ktwBnR4dW3A12xBks9tg9-NsNC8CJ5d_PKQ9juA-u2VPR1tshVOSUDfpqVqihGJGj2xM-Rva23Vewn4fNpGFEW-HKKNHMGjO4TvqFEb_E4ppASj6j6biG9H4vEL_vexxMpZyhijJSlPe6eSaIWscE9apUcgSh0ZiXV9TCsIL19ZxJ_B-Yr-DMuAWZTDMwBlFaikPG0zpcn-fnjlX6dQ8bWPmuDFJ6NdiExFKEAT_fV7xgiMBH-pVa6fI7w_THoQ
contexts:
# context is what you use - it describes for this cluster use this auth 
- context:
    cluster: k8s-digit-workshop--cluster
    user: k8s-digit-workshop--user
    namespace: urmet
  name: k8s-digit-workshop
```

</p>
</details>

Once done you can test if your `kubectl` is configured by using

```sh
kubectl --context=k8s-digit-workshop -n urmet auth can-i list pods
# should return yes
```

### About cluster

```sh
# list of all nodes for cluster 
kubectl --context=k8s-digit-workshop get nodes

# list of all namespaces for cluster
kubectl --context=k8s-digit-workshop get namespaces
```

## Workshop

### Checkout workshop files

Checkout this workshop guide locally:

```sh
git clone https://github.com/Veriff/k8s-digit-workshop.git
# move to folder where code was checked out
cd k8s-digit-workshop
# use your branch
git checkout urmet
```

### Run first container 

In example here and all the followings we are using [apache](https://hub.docker.com/_/httpd) docker images. 

> _Docker images are read-only templates used to build containers. Containers are deployed instances created from those templates._

```sh
# If you have docker installed locally you can test this image like this
# run apache docker and expose in port 8123, test with browser http://localhost:8123/
docker run --rm -p "8123:80" httpd
```

Now let's run this Apache docker image in Kubernetes using a [Pod](https://kubernetes.io/docs/concepts/workloads/pods/).

> _Pods are the smallest deployable units of computing that you can create and manage in Kubernetes._

> `kubectl --context=k8s-digit-workshop -n urmet apply -f examples/pod.yml`
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-example
  namespace: urmet
  labels:
    app: pod-example
    purpose: training
spec:
  containers:
  - name: web
    # apache docker image
    # https://hub.docker.com/_/httpd
    image: httpd
```

For testing, get list of pods in your namespace (`-n=urmet`)
```sh
# list all pods in your namespace
kubectl --context=k8s-digit-workshop -n=urmet get pods -o wide
```

> ![pods in Kubernetes cluster](/img/k8s-nodes-pods.png)

You can get more info about the object by using `kubectl get` and/or `kubectl describe`

```sh
# show declaration
kubectl --context=k8s-digit-workshop -n=urmet get pod/pod-example -o yaml

# describe pod
kubectl --context=k8s-digit-workshop -n=urmet describe pod/pod-example
```

If you want to delete the resource, use `kubectl delete`
```sh
# delete all resources from file
kubectl --context=k8s-digit-workshop -n=urmet delete -f examples/pod.yml

# or delete by name
kubectl --context=k8s-digit-workshop -n=urmet delete pod/pod-example
```

### Use deployment

[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) will provide a controller to manage Pods based of given specification.

> `kubectl --context=k8s-digit-workshop -n urmet apply -f examples/deployment.yml`
```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-example
  namespace: urmet
spec:
  # run 3 pods with same spec
  replicas: 3
  selector:
    matchLabels:
      # this select is used to join pods and deploy
      app: deploy-example
  template:
    metadata:
      labels:
        # those labels are used for all pods created by this deploy
        app: deploy-example
        purpose: training
    spec:
      containers:
      #
      # !!! spec of the pod starts here !!!
      #
      - name: deploy-example
        # docker image that is used in our pod
        image: httpd
        #
        # extra custom config for pod
        #
        ports:
        # this is the port that docker container exposes
        - containerPort: 80
        #
        # https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/
        #
        resources:
          #
          # requests is used to define the amount of resources that Kubernetes reserves for the pod
          #
          # NB: if Kubernetes (nodes) has no resources left, then they will not accept any new pods
          #
          requests:
            cpu: 50m
            memory: 100Mi
          #
          # limits will define what is the max amount of resources that cluster can give to the pod
          #
          limits:
            cpu: 300m
            memory: 500Mi
        # readinessProbe is used to check whether an application is ready to serve traffic
        readinessProbe:
          httpGet:
            path: /index.html
            port: 80
          initialDelaySeconds: 5
        # env values for pod
        env:
        - name: VAR_ONE
          value: "test env value for container"
```

```sh
# get list of deployments
kubectl --context=k8s-digit-workshop -n=urmet get deployments

# get list of pods
kubectl --context=k8s-digit-workshop -n=urmet get pods -o wide

# get deploy-example pods (-l is label selector, you can give it to all resource calls)
kubectl --context=k8s-digit-workshop -n=urmet get pods -l=app=deploy-example

# get all pods using shared purpose=training label selector
kubectl --context=k8s-digit-workshop -n=urmet get pods -l=purpose=training

# show labels in output (-L)
kubectl --context=k8s-digit-workshop -n=urmet get pods -L=purpose,app


#
# check offical kubectl cheatsheet for more examples of using selectors
# * https://kubernetes.io/docs/reference/kubectl/cheatsheet/#viewing-finding-resources
#


# connect into pod
kubectl --context=k8s-digit-workshop -n=urmet exec -it deployment/deploy-example -- sh
kubectl --context=k8s-digit-workshop -n=urmet exec -it <POD_NAME> sh

# show logs from pod
kubectl --context=k8s-digit-workshop -n=urmet logs -f --tail=20 -l=app=deploy-example
kubectl --context=k8s-digit-workshop -n=urmet logs -f --tail=20 <POD_NAME>


#
# try to delete one pod and see how deployment is spawning new one to keep required 3 pods always running
#

# get list of pods
kubectl --context=k8s-digit-workshop -n=urmet get pods 
# delete one pod
kubectl --context=k8s-digit-workshop -n=urmet delete pod <POD_NAME>
# and check list again
kubectl --context=k8s-digit-workshop -n=urmet get pods 
```


### Expose service

[Service](https://kubernetes.io/docs/concepts/services-networking/service/) gives you an abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

> `kubectl --context=k8s-digit-workshop -n urmet apply -f examples/service.yml`
```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: example1
  namespace: urmet
spec:
  # expose this application externally
  type: LoadBalancer
  # inside cluster access only
  #type: ClusterIP
  ports:
  - name: web
    port: 80
    targetPort: 80
    protocol: TCP
  selector:
    # this selector points to pods we want to target
    app: deploy-example
```

```sh
# get list of services
kubectl --context=k8s-digit-workshop -n=urmet get services

# get EXTERNAL-IP from output and try to open it
```

Service can be also used internally as service discovery / load-balance mechanism, so you will have URL inside the cluster you can access services with pattern `http://<service>:<port>/`

```sh
# connnect to pod
kubectl --context=k8s-digit-workshop -n=urmet exec -it deployment/deploy-example -- bash

# install curl
apt-get update && apt-get install -y curl

# test if you can access
curl http://example1:80/
```

### Ingress

> [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/): _An API object that manages external access to the services in a cluster, typically HTTP. Ingress may provide load balancing, SSL termination and name-based virtual hosting._

> `kubectl --context=k8s-digit-workshop -n urmet apply -f examples/ingress.yml`
```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: urmet
spec:
  ingressClassName: nginx
  rules:
  # https://urmet.digit.vrfgstest.org/
  - host: "urmet.digit.vrfgstest.org"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: example-ingress
            port:
              number: 80

---
apiVersion: v1
kind: Service
metadata:
  name: example-ingress
  namespace: urmet
spec:
  type: ClusterIP
  ports:
  - name: web
    port: 80
    targetPort: 80
    protocol: TCP
  selector:
    # this selector points to pods we want to target
    app: deploy-example
```

And try to access your app using [https://urmet.digit.vrfgstest.org/](https://urmet.digit.vrfgstest.org/).

```sh
# get list of ingress
kubectl --context=k8s-digit-workshop -n=urmet get ingress

# get list of services
kubectl --context=k8s-digit-workshop -n=urmet get services
```