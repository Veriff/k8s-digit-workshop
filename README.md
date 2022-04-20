# Kubernetes Workshop

 - [Setup kubectl](#setup-kubectl)
 - [Short intro](#short-intro)
 - [Run first container](#run-first-container)
 - [Use deployment](#use-deployment)
 - [Expose service](#expose-service)
 - [Ingress](#ingress)

Checkout this workshop guide locally:

```sh
git clone https://github.com/Veriff/k8s-workshop.git
cd k8s-workshop
# use your branch
git checkout aaaaaa
```

## Setup kubectl

First make sure you have `kubectl` (Kubernetes command-line tool) installed. In order to make it follow instructions in:
  - https://kubernetes.io/docs/tasks/tools/#kubectl

Once done add config to use our test Kubernetes cluster into `~/.kube/config` ([download](https://github.com/Veriff/k8s-workshop/raw/aaaaaa/kubectl-config)). 

```yaml
apiVersion: v1
kind: Config
current-context: k8s-digit-workshop
preferences: {}
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM1ekNDQWMrZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeU1EUXhNekU0TVRZeU5sb1hEVE15TURReE1ERTRNVFl5Tmxvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTHA5CmM4QytrNUI2VVQray9vRGt0b0NkbTk2NmRqNmVzSHhySzJiMFhHcTBQWGFtMFFWVlR0dTRuWlRvblJZWkZFZkcKMDVJS1RINmc4ZWIxaW9RcXZ3ZXNvWUVRaG5FaFBBek1uWlJyNnowOEpaWmdZQnpQcFpZeDJaTVlkbnFRYmZQRQo5NTRYVFgxVUt0ckVwWUw5Y2plNTNVVHdMeEpFNWpPRGRhcWNJbVFyMG1CMGs5UzdseEZob2lYaE1RMVUzdENUClA2ajhETzBRV1VVbUJEWkc1SkRqcWQ0ckVJckhscFhxa2RiZ1lCMksxMDQ5U2gzMmsrVjduVHJvbjA5d0FHc3EKKzdWNHQxZjFraW9IWXlWZWVjMG16Y0gvSXRCeUFyZkFEQmQ4ZmhIdlorYlV6SnFXVk95ZW0rRThFb083S2pDZwpESkpqOFpqcEdCQnNqNkZzNzZVQ0F3RUFBYU5DTUVBd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZDSXFWdTQ2dHF3b0ZsN2I3MmNyNW56dHBENEZNQTBHQ1NxR1NJYjMKRFFFQkN3VUFBNElCQVFBNnVmR2xQcTYreEhrVTBTc3NmS3ZsZkQ2UXJuWmJWSzFic1dmRHkxUFBzOWVEMSszcwozNmMwMWZGU0hBVlJDMXBIUmlsYkFudXBScEppRGxHK1dOR3dEckJ3TEd6Z0tZdE81NkFrbFo1UHRib2licW10CjRJY2h2cVN4ZEw1NXhwS3RmZWdqZnhNa2lKaDd1d1NvMGt4cjE0N3NDV2x0ZmM5aGFEVkZKdUJrOVNWU2ZXSlcKemhVZVRwcDNLSGdRamJqS0szV09mS3c3YkNJdFVMemNWalRJeFBObTJ6VmhmQWR1dFhBM3h3ZURvMVZYMDZXawo1Y1lyZkd5WVVwSGZTUFZWbkRNM3p3Kzc0aXNKWmd4aWQ1ME11Z3lDcEliRlRoeURMaG9pb0oyMW9pNVVraXM1CkF0RHhqL3VYTjdhNVZRSGc4NUZUdVNBcTl2dXRZZDRWaEliQwotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
    server: https://14AE70BA3791C735370E9252575A533A.yl4.eu-north-1.eks.amazonaws.com
  name: k8s-digit-workshop--cluster
contexts:
- context:
    cluster: k8s-digit-workshop--cluster
    user: k8s-digit-workshop--user
    namespace: aaaaaa
  name: k8s-digit-workshop
users:
- name: k8s-digit-workshop--user
  user:
    token: eyJhbGciOiJSUzI1NiIsImtpZCI6InV6dkF1NUN6U0tiLUplTUQtbXl0WHFZRU5INWxrLWdwbEVLLV9rTlAzZUEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJhYWFhYWEiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlY3JldC5uYW1lIjoiYWFhYWFhLXRva2VuLWd6OHNkIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFhYWFhYSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImY2YzcyYTllLWNkYWItNGNhYi1iMzY1LWNhYTY2MzA1YTg1YyIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDphYWFhYWE6YWFhYWFhIn0.Yc5Z2CnwrDj0ntJVozXeBAKrTe-se3d1qzOVj1nTo_XDr9OevtlfHna6gAYexHz4Cp8ujctYX87v1875nYw2gTIRt_5D8HZmgcgedeSRq0R8JD8pUPaEiFqxEjl8xSgiILjUhDXntgk78QJ3oBXWNWm22PY0XnusEn1OctQ8_u-9yFNOyP4zDdIVE3NQ7IvbqwLhA_WYsVJEW4peL5X5fCG9hhYzDfXSjyy_5ZuMbnVLkzuGBo_vQpwMHwBo_l-eCO1zIIG1zGDg1jTuQfZ_2oQM4hUwQL2NVzWT2UZklmfP3KJsdZpM6rhLSDS16Gg6JUrN7QwPT2gtJGNxmdEvAQ

```

Once done you can test if your `kubectl` is configured by using

```sh
kubectl --context=k8s-digit-workshop -n aaaaaa auth can-i list pods
```

## Short intro



## Run first container 

In example here and all the followings we are using [apache](https://hub.docker.com/_/httpd) docker images. 

```sh
# If you have docker installed locall you can test this image like this
# run apache docker and expose in port 8123, test with browser http://localhost:8123/
docker run --rm -p "8123:80" httpd
```

Now let's run this Apache docker image in Kubernetes.

> `kubectl --context=k8s-digit-workshop -n aaaaaa apply -f examples/pod.yml`
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-example
  namespace: aaaaaa
  labels:
    app: pod-example
    purpose: training
spec:
  containers:
  - name: web
    # apache docker image
    # https://hub.docker.com/_/httpd
    image: httpd:latest
```

For testing, get list of pods in your namespace (`-n=aaaaaa`)
```sh
# list all pods in your namespace
kubectl --context=k8s-digit-workshop -n=aaaaaa get pods
```

You can get more info about the object by using `kubectl get` and/or `kubectl describe`

```sh
# show declaration
kubectl --context=k8s-digit-workshop -n=aaaaaa get pod/pod-example -o yaml

# describe pod
kubectl --context=k8s-digit-workshop -n=aaaaaa describe pod/pod-example
```

If you want to delete the resource, use `kubectl delete`
```sh
# delete all resources from file
kubectl --context=k8s-digit-workshop -n=aaaaaa delete -f examples/pod1.yml

# or delete by name
kubectl --context=k8s-digit-workshop -n=aaaaaa delete pod/pod-example
```

## Use deployment

Deployment will manage to one or more replicas of pod.

> `kubectl --context=k8s-digit-workshop -n aaaaaa apply -f examples/deployment.yml`
```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-example
  namespace: aaaaaa
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
        image: httpd:latest
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
            cpu: 10m
            memory: 10Mi
          #
          # limits will define what is the max amount of resources that cluster can give to the pod
          #
          limits:
            cpu: 20m
            memory: 20Mi
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
kubectl --context=k8s-digit-workshop -n=aaaaaa get deployments

# get list of pods
kubectl --context=k8s-digit-workshop -n=aaaaaa get pods

# get deploy-example pods (-l is label selector, you can give it to all resource calls)
kubectl --context=k8s-digit-workshop -n=aaaaaa get pods -l=app=deploy-example

# get all pods using shared purpose=training label selector
kubectl --context=k8s-digit-workshop -n=aaaaaa get pods -l=purpose=training

# show labels in output (-L)
kubectl --context=k8s-digit-workshop -n=aaaaaa get pods -L=purpose,app


#
# check offical kubectl cheatsheet for more examples of using selectors
# * https://kubernetes.io/docs/reference/kubectl/cheatsheet/#viewing-finding-resources
#


# connect into pod
kubectl --context=k8s-digit-workshop -n=aaaaaa exec -it deployment/deploy-example -- sh
kubectl --context=k8s-digit-workshop -n=aaaaaa exec -it <POD_NAME> sh

# show logs from pod
kubectl --context=k8s-digit-workshop -n=aaaaaa logs -f --tail=20 -l=app=deploy-example
kubectl --context=k8s-digit-workshop -n=aaaaaa logs -f --tail=20 <POD_NAME>


#
# try to delete one pod and see how deployment is spawning new one to keep required 3 pods always running
#

# get list of pods
kubectl --context=k8s-digit-workshop -n=aaaaaa get pods 
# delete one pod
kubectl --context=k8s-digit-workshop -n=aaaaaa delete pod <POD_NAME>
# and check list again
kubectl --context=k8s-digit-workshop -n=aaaaaa get pods 
```


## Expose service

[Service](https://kubernetes.io/docs/concepts/services-networking/service/) gives you an abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

> `kubectl --context=k8s-digit-workshop -n aaaaaa apply -f examples/service.yml`
```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: example1
  namespace: aaaaaa
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
kubectl --context=k8s-digit-workshop -n=aaaaaa get services

# get EXTERNAL-IP from output and try to open it

```

Service can be also used internally as service discovery / load-balance mechanism.

## Ingress


