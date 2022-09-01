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
 - [More reading](#more-reading)

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

Once done add config to use our test Kubernetes cluster into `~/.kube/config` ([download](https://github.com/Veriff/k8s-workshop/raw/vrf-raigo-jurisoo/kubectl-config)). 


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
current-context: k8s-workshop
preferences: {}
clusters:
# cluster info
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeU1EZ3pNVEl3TlRNeU9Gb1hEVE15TURneU9ESXdOVE15T0Zvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTnc3ClRrS2pSem1KaEcxL0luYmtZT2dJc212MTRZeUxRSm8xM0tpZm5WUFZaSUhsNlBDZVF6UFZ5NDRxNmJJZ0wzakcKMk0zSUMrKzBlL1lTblRHMVRoNnhhQmpkK3hmb1VDa2xzT0g2QkVtWWd4N2hpdVcrUjAzeXcxTTd1NzBkZ2FUZgpvY2pCcjJScUZsTS95dE1obkZDd243dEE2MUU3T1JkSXluRXdjRE0wRFZ2dWtWZG9wWCtaNVQwZVZNWkJ6UWp0CkRjRGpyT2xxZ2xQTldtc25rL1I1Zm1xcERWVDJ2UDArRGpNbHNFT3cyNW1LZitSVTdZSkJuRXJTbjlUd1hWOHIKTHdwU0RXdjg2Mk5CRXlua2pWS01BTnRRT0lOQ0RYY3l0aDJFS0pBbnN1czNidDY3RE9hK2NuRWNOdEhSM3NiRgo4dEI2U3g1SkFJb3Y2bWxMemRFQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZOSy9LeXJ4dTQ4NFpWMzNERURCcUVGbWlOWkVNQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBSnZQYW1UZzF0eUZoSWtORm03Kwp5SWYyRHFjblhPVU43Yk5lcXpGeklob0tOK2Uwak9rQXRsNVMrWm9nZE1GRjZUazhDVVdrVGY3UGp5ZTkvOEFjClIxU3U2b0NZU25IVFUzLzR6UlBLdkdadWVLRk13N0dBVi9ad0x3bi8wUFFqamk5b2JjTHVLNDVWL280QWRZTHMKb3krQSsySUNQYXlENENhT3RjamgwWGZUQmoxemQreXlqQmlVUHk3SUFPVjVmbURGbExWc2phTFdScksyR3NKVgp0QVp3ckFmYzNZYS9sWTEzZUR6cGcxTmw1blg5VUo4aHFZVG8yamM4Sk9mUU80YytpUTVuQUc3OWxJWU12d3FCClFoaEhnbWVVMGFIb3ZVTU5QeXRLYVNra2YxYU0zczR4dmJNNFRiaStxZ1Q3OE5uRVZScHhSV2U5K0gzRGthTlAKVDBrPQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    server: https://0D25DDBAE523E88E7A76F37E50113F60.yl4.eu-north-1.eks.amazonaws.com
  name: k8s-workshop--cluster
users:
# auth info
- name: k8s-workshop--user
  user:
    token: eyJhbGciOiJSUzI1NiIsImtpZCI6IjZSMHhwYm1LN3d2Sk1RUUJFaXhOQ2JqZEJocDZsc1JkYmZhM2czZUxhVm8ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJ2cmYtcmFpZ28tanVyaXNvbyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJ2cmYtcmFpZ28tanVyaXNvby10b2tlbi1yeDZsYiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJ2cmYtcmFpZ28tanVyaXNvbyIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImJmZWIwZDkzLWRkNjUtNGEwZS1iMzE5LTE4NjBkYzBmNTBkOCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDp2cmYtcmFpZ28tanVyaXNvbzp2cmYtcmFpZ28tanVyaXNvbyJ9.muLxGy-7kc4rHPxpbCqZfZi4Q3uBHb3quRgNd6Z72309vdUA5bx980yWefWJihdvwhr9DusIG8FA-vTYPyhZGXpHmIiLusTLkTs7VLYKVccpPhjsms5ZTgsX4uFacFNe0sXZZWqCZYrHj14slTvY4wje1Te6GJxpI3eVBcphgj3HP0BTcdtV6fzwubxcF2vskSUqi8RI6Z0YyfM4WpOt8LyetmUiuGuVxVlCwv5GXQyo6trEt-BGVhSniszLimvatLlSZzMe5xwqZ3KoX8wtf5-Mcnwp01RGNYX7JUfVK2eUVlWK_ivVpwqnpLsiISm_eDJHll3JHxPesqSYYs6RIQ
contexts:
# context is what you use - it describes for this cluster use this auth 
- context:
    cluster: k8s-workshop--cluster
    user: k8s-workshop--user
    namespace: vrf-raigo-jurisoo
  name: k8s-workshop

```

</p>
</details>

Once done you can test if your `kubectl` is configured by using

```sh
kubectl --context=k8s-workshop -n vrf-raigo-jurisoo auth can-i list pods
# should return yes
```

### About cluster

```sh
# list of all nodes for cluster 
kubectl --context=k8s-workshop get nodes

# list of all namespaces for cluster
kubectl --context=k8s-workshop get namespaces
```

## Workshop

### Checkout workshop files

Checkout this workshop guide locally:

```sh
git clone https://github.com/Veriff/k8s-digit-workshop.git
# move to folder where code was checked out
cd k8s-digit-workshop
# use your branch
git checkout vrf-raigo-jurisoo
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

> `kubectl --context=k8s-workshop -n vrf-raigo-jurisoo apply -f examples/pod.yml`
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-example
  namespace: vrf-raigo-jurisoo
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

For testing, get list of pods in your namespace (`-n=vrf-raigo-jurisoo`)
```sh
# list all pods in your namespace
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pods -o wide
```

> ![pods in Kubernetes cluster](/img/k8s-nodes-pods.png)

You can get more info about the object by using `kubectl get` and/or `kubectl describe`

```sh
# show declaration
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pod/pod-example -o yaml

# describe pod
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo describe pod/pod-example
```

If you want to delete the resource, use `kubectl delete`
```sh
# delete all resources from file
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo delete -f examples/pod.yml

# or delete by name
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo delete pod/pod-example
```

### Use deployment

[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) will provide a controller to manage Pods based of given specification.

> `kubectl --context=k8s-workshop -n vrf-raigo-jurisoo apply -f examples/deployment.yml`
```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-example
  namespace: vrf-raigo-jurisoo
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
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get deployments

# get list of pods
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pods -o wide

# get deploy-example pods (-l is label selector, you can give it to all resource calls)
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pods -l=app=deploy-example

# get all pods using shared purpose=training label selector
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pods -l=purpose=training

# show labels in output (-L)
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pods -L=purpose,app


#
# check offical kubectl cheatsheet for more examples of using selectors
# * https://kubernetes.io/docs/reference/kubectl/cheatsheet/#viewing-finding-resources
#


# connect into pod
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo exec -it deployment/deploy-example -- sh
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo exec -it <POD_NAME> sh

# show logs from pod
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo logs -f --tail=20 -l=app=deploy-example
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo logs -f --tail=20 <POD_NAME>


#
# try to delete one pod and see how deployment is spawning new one to keep required 3 pods always running
#

# get list of pods
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pods 
# delete one pod
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo delete pod <POD_NAME>
# and check list again
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get pods 
```


### Expose service

[Service](https://kubernetes.io/docs/concepts/services-networking/service/) gives you an abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

> `kubectl --context=k8s-workshop -n vrf-raigo-jurisoo apply -f examples/service.yml`
```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: example1
  namespace: vrf-raigo-jurisoo
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
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get services

# get EXTERNAL-IP from output and try to open it
```

Service can be also used internally as service discovery / load-balance mechanism, so you will have URL inside the cluster you can access services with pattern `http://<service>:<port>/`

```sh
# connnect to pod
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo exec -it deployment/deploy-example -- bash

# install curl
apt-get update && apt-get install -y curl

# test if you can access
curl http://example1:80/
```

### Ingress

> [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/): _An API object that manages external access to the services in a cluster, typically HTTP. Ingress may provide load balancing, SSL termination and name-based virtual hosting._

> `kubectl --context=k8s-workshop -n vrf-raigo-jurisoo apply -f examples/ingress.yml`
```yaml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
  namespace: vrf-raigo-jurisoo
spec:
  ingressClassName: nginx
  rules:
  # https://vrf-raigo-jurisoo.k8s.vrfgstest.org/
  - host: "vrf-raigo-jurisoo.k8s.vrfgstest.org"
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
  namespace: vrf-raigo-jurisoo
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

And try to access your app using [https://vrf-raigo-jurisoo.k8s.vrfgstest.org/](https://vrf-raigo-jurisoo.k8s.vrfgstest.org/).

```sh
# get list of ingress
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get ingress

# get list of services
kubectl --context=k8s-workshop -n=vrf-raigo-jurisoo get services
```

## More reading

Number of more commonly used and useful k8s resources:
 
 - Config managment for your applications:
    - [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/): _Senstive data like tokens, passwords etc_
    - [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/): _None-senstive data in key-value map_
 - [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/): _Manage Pods with state (including persistent storage)_
 - [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/): _Execute Pods (jobs) with repeating [crontab](https://crontab.guru/) schedule_
 - [RBAC Authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/): _Control who can do what in your clusters_
 - [NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/): _Add networking policies (example: firewalling) for your deployments_
 - [Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/): _In Kubernetes, namespaces provides a mechanism for isolating groups of resources within a single cluster_
 - ... and so many other options, check [offical kubernetes docs](https://kubernetes.io/) ...
