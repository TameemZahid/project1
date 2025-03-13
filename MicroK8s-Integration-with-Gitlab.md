# Microk8s Integration with GitLab 


> [!NOTE]
> This document is prepared with the help of [official documentation](https://docs.gitlab.com/runner/install/kubernetes/) 


**Installing Helm chart**

`snap install helm –classic`


**Add the GitLab Helm repository** 

`helm repo add gitlab https://charts.gitlab.io`


**Update the chart**

`helm repo update gitlab`


**Check which GitLab Runner versions you have access to**

`helm search repo -l gitlab/gitlab-runner` 

 

**sample output:**

```
NAME                    CHART VERSION   APP VERSION     DESCRIPTION 

gitlab/gitlab-runner    0.74.0          17.9.0          GitLab Runner 

gitlab/gitlab-runner    0.73.3          17.8.3          GitLab Runner 

gitlab/gitlab-runner    0.73.2          17.8.2          GitLab Runner 

gitlab/gitlab-runner    0.73.1          17.8.1          GitLab Runner 

gitlab/gitlab-runner    0.73.0          17.8.0          GitLab Runner 

gitlab/gitlab-runner    0.72.1          17.7.1          GitLab Runner 

gitlab/gitlab-runner    0.72.0          17.7.0          GitLab Runner 
```

 

**Configure GitLab Runner in values.yaml file**

> [!NOTE]
> The file is created with configurations, with the name **helm_values.yaml**  

  

`root@gitlab-test:~/osfp-gitlab-runner# cat helm_values.yaml`

```
concurrent: 5 

logFormat: json 

rbac: 

  create: true 

  rules: 

    - apiGroups: [""] 

      resources: ["configmaps", "events", "pods", "pods/attach", "pods/exec", "secrets", "services"] 

      verbs: ["get", "list", "watch", "create", "patch", "update", "delete"] 

runners: 

  config: | 

    [[runners]] 

      [runners.kubernetes] 

        namespace = "{{.Release.Namespace}}" 

        image = "alpine" 
```
 

**Create a Runner in GitLab**

1. Create a runner in GitLab by navigating to the “Runners” menu in the admin area. 

2. Get the following command to register the runner.  

`gitlab-runner register  --url https://<Your-URL>  --token <Your-Token>`

> [!IMPORTANT]
> Here we should have information about two things, i.e. GitLab URL and token, these will be used in the next step. 

 

**Apply the Helm chart**

Apply the helm chart with following command  

 
```
microk8s.helm install --namespace gitlab-runner --create-namespace --atomic --timeout 120s --set gitlabUrl=https://<Your-URL> --set runnerToken=<Your-Token> --values helm_values.yaml gitlab-runner gitlab/gitlab-runner --version 0.74.0 
```
 

**Verify the Runner in Microk8s cluster**

`microk8s.kubectl get pod -n gitlab-runner`

```
NAME                             READY   STATUS    RESTARTS   AGE 

gitlab-runner-7f957d884f-frb59   1/1     Running   0          5m51s
```


# Deploy Simple Nginx App using GitLab CI/CD Pipeline  


Create a deployment file for the app, within the repo with the name **kubernetes/deployment.yaml**.  

 

 
```
apiVersion: apps/v1 

kind: Deployment 

metadata: 

  name: my-app 

  labels: 

    app: my-app 

spec: 

  replicas: 1 

  selector: 

    matchLabels: 

      app: my-app 

  template: 

    metadata: 

      labels: 

        app: my-app 

    spec: 

      containers: 

      - name: my-app 

        image: nginx:1.14.2 

        ports: 

        - containerPort: 80 
```
 

 


**Store environment variables in the repo/group settings of CI/CD**

```
MICROK8S_IP_ADDRESS: 159.223.106.47 

MICROK8S_USERNAME: root 

MICROK8S_PASSWORD: <Your-Password> 
```


**Write a basic CICD pipeline with a single stage to demonstrate deployment on Microk8s cluster.**

Within the repo create a file **.gitlab-ci.yml**, having following contents inside.  

> [!IMPORTANT]
> The file name must start with a dot(.)
> .gitlab-ci.yml

 
```
stages: 

  - deploy 

 

 

deploy-job: 

  stage: deploy 

  image: alpine:latest  # Use Alpine image 

  before_script: 

    - apk add --no-cache openssh sshpass 

  script: 

    - sshpass -p "$MICROK8S_PASSWORD" scp -o StrictHostKeyChecking=no kubernetes/deployment.yaml ${MICROK8S_USERNAME}@${MICROK8S_IP_ADDRESS}:/${MICROK8S_USERNAME}/deployment.yaml


    - sshpass -p "$MICROK8S_PASSWORD" ssh -o StrictHostKeyChecking=no ${MICROK8S_USERNAME}@${MICROK8S_IP_ADDRESS} "cd /${MICROK8S_USERNAME} && microk8s.kubectl apply -f deployment.yaml" 

  tags: 

  - microk8s 
```
