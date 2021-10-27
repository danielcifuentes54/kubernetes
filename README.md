# Kubernetes Commands

## Commons

> Create, replace and update an object from yml file.
> - kubectl create -f <PATH_FILE>
> - kubectl replace -f <PATH_FILE>
> - kubectl apply -f <PATH_FILE>

> Get pods, replicaset, deployments and services objects.
> - kubectl get all

> Get all objects from a resource.
> - kubectl get <RESOURCE_NAME>
>   - `kubectl get pods`

> Describe an object from a resource.
> - kubectl describe <RESOURCE_NAME> <OBJECT_NAME>
>   - `kubectl describe pod nginx`

> Edit an object from a resource.
> - kubectl edit <RESOURCE_NAME> <OBJECT_NAME>
>   - `kubectl edit pod nginx`

> Delete an object from a resource.
> - kubectl delete <RESOURCE_NAME> <OBJECT_NAME>
>   - `kubectl delete pod nginx`


## Pods (Pod)

> Create a pod with a single container 
> - kubectl run <POD_NAME> --image <CONTAINER_IMAGE>
>   - `kubectl run nginx --image nginx`
>   - Options:
>       - add labels
>           - kubectl run <POD_NAME> --image <CONTAINER_IMAGE> --labels <MY_KEY>=<MY_VALUE>
>           - `kubectl run nginx --image nginx --labels my-key=my-value`


## Replicaset (rs)

> Scale replicaset
> - kubectl scale rs <RS_NAME> --replicas <REPLICAS_NUMBER>
>   - `kubectl scale rs my-rs --replicas 5`

## Deployment (deploy)

> Create a deployment 
> - kubectl create deploy <DEPLOY_NAME> --image <CONTAINER_IMAGE>
>   - `kubectl create deploy my-deploy --image nginx`

> Get the rollout status
> - kubectl rollout status deploy <DEPLOY_NAME>
>   - `kubectl rollout status deploy my-deploy`

> Get the rollout history
> - kubectl rollout history deploy <DEPLOY_NAME>
>   - `kubectl rollout history deploy my-deploy`

> Set image
> - kubectl set image deploy <DEPLOY_NAME> <CONTAINER_NAME>=<IMAGE_NAME> 
>   - `kubectl set image deploy my-deploy my-web-app=nginx`

> Undo rollout
> - kubectl rollout undo deploy <DEPLOY_NAME> 
>   - `kubectl rollout undo deploy my-deploy`

## Services (svc)

> Create a service (ClusterIP) from pod, this option use the pod labels as selectors
> - kubectl expose pod <POD_NAME> --port <TARGET_PORT> --name <SERVICE_NAME>
>   - `kubectl expose pod my-pod --port 80 --name my-ci-service`

> Create a ClusterIP service
> - kubectl create service clusterip <SERVICE_NAME> --tcp <CLUSTER_PORT>:<TARGET_PORT>
>   - `kubectl create service clusterip my-ci-service --tcp=80:8080`

> Create a NodePort service
> - kubectl create service nodeport <SERVICE_NAME> --tcp <CLUSTER_PORT>:<TARGET_PORT> --node-port <NODE_PORT>
>   - `kubectl create service nodeport my-np-service --tcp 80:80 --node-port 30000`
      

## Namespaces (ns)

> Create a new namespace
> - kubectl create ns <NAMESPACE_NAME>
>   - `kubectl create ns my-namespace`

> Execute any command with a specific namespace
> - <ANY_COMMAND> -n <NAMESPACE_NAME>
>   - `kubectl get pods -n my-namespace`

> Execute any command with all the namespaces
> - <ANY_COMMAND> --all-namespaces
>   - `kubectl get pods --all-namespaces`

> Set namespace in the context
> - kubectl config set-context $(kubectl config current-context) --namespace=<NAMESPACE_NAME>
>   - `kubectl config set-context $(kubectl config current-context) --namespace=my-ns`

## Options

> know how to create a definition file from an imperative command
> - <IMPERATIVE_COMMAND> --dry-run=client -o <OUTPUT_FORMAT>
>   - `kubectl run nginx --image nginx --dry-run=client -o yaml`



