# Kubernetes Workshop

 - [Setup kubectl](#setup-kubectl)
 - Run docker container in k8s

Checkout this workshop guide locally:

```sh
git clone https://github.com/Veriff/k8s-workshop.git
cd k8s-workshop
# use your branch
git checkout yyy
```

## Setup kubectl

First make sure you have `kubectl` (Kubernetes command-line tool) installed. In order to make it follow instructions in:
  - https://kubernetes.io/docs/tasks/tools/#kubectl

Once done add config to use our test Kubernetes cluster into `~/.kube/config` ([download](https://github.com/Veriff/k8s-workshop/raw/yyy/kubectl-config)). 

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
    namespace: yyy
  name: k8s-digit-workshop
users:
- name: k8s-digit-workshop--user
  user:
    token: uu

```

Once done you can test if your `kubectl` is configured by using

```sh
kubectl --context=k8s-digit-workshop auth can-i list pods
```