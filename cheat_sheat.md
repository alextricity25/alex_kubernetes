I used minikube with the virtualbox driver on windows to build a all-in-one
kubernetes cluster

`minikube.exe start`
https://github.com/kubernetes/minikube/issues/3900
`minikube.exe start --no-vtx-check`


# Namespaces

`kubectl get namespaces`

# Deployments

`kubectl create deployment mynginx --image=nginx:1.15-alpine`

`kubectl get deploy,rs,po -l app=mynginx`

# Scaling

`kubectl scale deploy mynginx --replicas=3`

# Rollouts

## Set another image

`kubectl set image deployment mynginx nginx=nginx:1.16-alpine`

## See history

`kubectl rollout history deploy mynginx`

# Deploying the dashboard using minikube
`minikube dashboard`

# Rediness Probes
Sometimes, while initializing, applications have to meet certain conditions before they become ready to serve traffic. These conditions include ensuring that the depending service is ready, or acknowledging that a large dataset needs to be loaded, etc. In such cases, we use Readiness Probes and wait for a certain condition to occur. Only then, the application can serve traffic.

# Liveness Probe
If a container in the Pod has been running successfully for a while, but the application running inside this container suddenly stopped responding to our requests, then that container is no longer useful to us. This kind of situation can occur, for example, due to application deadlock or memory pressure. In such a case, it is recommended to restart the container to make the application available.

Rather than restarting it manually, we can use a Liveness Probe. Liveness probe checks on an application's health, and if the health check fails, kubelet restarts the affected container automatically.

# Volumes


# Check services and enpoints
`kbuectl get services,endpoints`

# ConfigMaps
ConfigMaps allow us to decouple the configuration details from the container image. Using ConfigMaps, we pass configuration data as key-value pairs, which are consumed by Pods or any other system components and controllers, in the form of environment variables, sets of commands and arguments, or volumes. We can create ConfigMaps from literal values, from configuration files, from one or more files or directories.

## Creating a ConfigMap
`kubectl create configmap my-config --from-literal=key1=values1 --from-literal=key2=value2`

## Display the ConfigMap Details
`kubectl get configmaps my-config -o yaml`

# Secrets
Let's assume that we have a Wordpress blog application, in which our wordpress frontend connects to the MySQL database backend using a password. While creating the Deployment for wordpress, we can include the MySQL password in the Deployment's YAML file, but the password would not be protected. The password would be available to anyone who has access to the configuration file.

In this scenario, the Secret object can help by allowing us to encode the sensitive information before sharing it. With Secrets, we can share sensitive information like passwords, tokens, or keys in the form of key-value pairs, similar to ConfigMaps; thus, we can control how the information in a Secret is used, reducing the risk for accidental exposures. In Deployments or other resources, the Secret object is referenced, without exposing its content.

It is important to keep in mind that by default, the Secret data is stored as plain text inside etcd, therefore administrators must limit access to the API server and etcd. However, Secret data can be encrypted at rest while it is stored in etcd, but this feature needs to be enabled at the API server level.

## Creating a secret
`kubectl create secret generic my-password --from-literal=passowrd=mysqlpassword`

## Describe secret - Does not reveal the content of Secret
`kubectl get secret my-password`

# Look into the container
`kubectl exec app-config -- /bin/sh -c 'cat /usr/share/nginx/htmls/index.html'`
