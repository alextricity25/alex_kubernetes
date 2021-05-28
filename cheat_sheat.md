I used minikube with the virtualbox driver on windows to build a all-in-one
kubernetes cluster

`minikube.exe start`


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
