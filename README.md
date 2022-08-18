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
>   - `kubectl delete pod nginx --grace-period=0 --force`


## Pods (Pod)

> Create a pod with a single container 
> - kubectl run <POD_NAME> --image <CONTAINER_IMAGE>
>   - `kubectl run nginx --image nginx`
>   - Options:
>       - add labels
>           - kubectl run <POD_NAME> --image <CONTAINER_IMAGE> --labels <MY_KEY>=<MY_VALUE>
>           - `kubectl run nginx --image nginx --labels my-key=my-value`

> Get a shell to a running container
> kubectl exec <POD_NAME> -it -- <MY_COMMAND>
> - `kubectl exec my-pod -it -- bash`

> Get logs from a pod
> kubectl logs <POD_NAME> <CONTAINER_NAME>
> - `kubectl logs my-pod my-container`

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
> - kubectl set image deploy <DEPLOY_NAME> <CONTAINER_NAME>=<IMAGE_NAME> --record
>   - `kubectl set image deploy my-deploy my-web-app=nginx --record`
> the flag `--record` is optional, add it if you want to write the command executed in the resource annotation `kubernetes.io/change-cause`

> Undo rollout
> - kubectl rollout undo deploy <DEPLOY_NAME> --to-revision <REVISION_NUMBER>
>   - `kubectl rollout undo deploy my-deploy --to-revision 1`
> the flag `--to-revision` is optional, add it if you want to go to a specific revision


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

## Configmaps (cm)

> Create a new configmap from literals
> - kubectl create cm <CONFIGMAP_NAME> --from-literal=<MY_KEY>=<MY_VALUE> --from-literal=<MY_KEY_2>=<MY_VALUE_2>
>   - `kubectl create cm my-configmap --from-literal=ANY_KEY=ANY_VALUE --from-literal=ANY_KEY_2=ANY_VALUE_2`

> Create a new configmap from a file
> - kubectl create cm <CONFIGMAP_NAME> --from-file=<FILE_PATH>
>   - `kubectl create cm my-configmap --from-file=/tmp/configmap.properties`


## Secrets (secret)

> Create a new secret from literals
> - kubectl create secret generic <SECRET_NAME> --from-literal=<MY_KEY>=<MY_VALUE> --from-literal=<MY_KEY_2>=<MY_VALUE_2>
>   - `kubectl create secret generic my-secret --from-literal=ANY_KEY=ANY_VALUE --from-literal=ANY_KEY_2=ANY_VALUE_2`

> Create a new secret from a file
> - kubectl create secret generic <SECRET_NAME> --from-file=<FILE_PATH>
>   - `kubectl create secret generic my-secret --from-file=/tmp/secrets.properties`

> Create a new TLS secret
> - kubectl create secret tls <TLS_SECRET_NAME> --cert <CERT_PATH> --key <KEY_PATH>
>   - `kubectl create secret tls webhook-server-tls --cert "/root/keys/webhook-server-tls.crt" --key "/root/keys/webhook-server-tls.key"`


## Service Account (sa)

> Create a new service account (this command also create a new secret for the service account)
> - kubectl create sa <SERVICE_ACCOUNT_NAME>
>   - `kubectl create sa my-service-account`

## Taints

> Associate a taint to a node
> - kubectl taint node <NODE_NAME> <MY_KEY>=<MY_VALUE>:<TAINT_EFFECT>
>   - `kubectl taint node my-node my-key=my-value:NoSchedule`
>
> Taints effects:
> - NoSchedule
> - PreferNoSchedule
> - NoExecute

> delete a taint from a node
> - kubectl taint node <NODE_NAME> <MY_KEY>-
>   - `kubectl taint node my-node my-key-`

## Node Selectors

> Associate a label to a node
> - kubectl label node <NODE_NAME> <MY_KEY>=<MY_VALUE>
>   - `kubectl label node my-node my-key=my-value`

## Jobs (job)

> Create a new job
> - kubectl create job <JOB_NAME> --image <IMAGE_NAME>
>   - `kubectl create job my-job --image busy-box`

## Cron-Jobs (cj)

> Create a new job
> - kubectl create cj <CRON_JOB_NAME> --schedule="<SCHEDULE_PATTERN>" --image <IMAGE_NAME>
>   - `kubectl create cj my-cron-job --schedule="1 * * * *" --image busy-box`

## Ingress 

> Create a new ingress resource
> -  kubectl create ingress <INGRESS_NAME> --rule <INGRESS_RULE>
>   - `kubectl create ingress my-ingress --rule="foo.com/bar=svc1:8080,tls=my-cert"`

## Kube Config 

> Get current kube config
> -  kubectl config view
>   - `kubectl config view`

> Set a Kube Config file
> -  kubectl config view --kubeconfig <MY_CUSTOM_CONFIG_PATH>
>   - `kubectl config view --kubeconfig ~/Documents/kube-config.yml"`

> Get current context
> -  kubectl config current-context
>   - `kubectl config current-context`

> Change context
> -  kubectl config use-context <CONTEXT_NAME>
>   - `kubectl config use-context dev-user@development`

> Set credentials
> -  kubectl config set-credentials <USER_NAME> --client-certificate <CRT_PATH> --client-key <KEY_PATH>
>   - `kubectl config set-credentials drogo --client-certificate /root/drogo.crt --client-key /root/drogo.key`

## role

> create role
> -  kubectl create role <ROLE_NAME> --verb <VERB_NAME> --resource <RESOURCES_TO_BIND>
>   - `kubectl create role developer --verb list,create,delete --resource pods`
>   - Options:
>       - Role for specific resource
>           - kubectl create role <ROLE_NAME> --verb <VERB_NAME> --resource <RESOURCES_TO_BIND> --resourcename <RESOURCE_NAME>
>           - `kubectl create role developer --verb list,create,delete --resource pods --resourcename my-pod`

## rolebinding 

> create rolebinding
> -  kubectl create rolebinding <ROLEBINDING_NAME> --role <ROLE_NAME> --user <USER_NAME>
>   - `kubectl create rolebinding dev-user:developer --role developer --user dev-user`

## clusterrole

> create clusterrole
> -  kubectl create clusterrole <CLUSTER_ROLE_NAME> --verb <VERB_NAME> --resource <RESOURCES_TO_BIND> 
>   - `kubectl create clusterrole node-admin --verb get --resource nodes `

## clusterrolebinding 

> create rolebinding
> -  kubectl create clusterrolebinding <ROLEBINDING_NAME> --clusterrole <CLUSTER_ROLE_NAME> --user <USER_NAME>
>   - `kubectl create clusterrolebinding dev-user:node-admin --clusterrole node-admin --user dev-user`

## AUTH

> Check access
> -  kubectl auth can-i <ACTION_TO_VALIDATE>
>   - `kubectl auth can-i create deployments`
 

## Options

> Know how to create a definition file from an imperative command
> - <IMPERATIVE_COMMAND> --dry-run=client -o <OUTPUT_FORMAT>
>   - `kubectl run nginx --image nginx --dry-run=client -o yaml`

> Know how to use any resource
> kubectl explain <RESOURCE_NAME> --recursive
> - `kubectl explain pods --recursive`

> Show the labels from any resource
> - <KUBECTL_COMMAND> --show-labels
>   - `kubectl get pods --show-labels`
>
> Filter any resource by the label
> - <KUBECTL_COMMAND> -l <MY_KEY_TO_FILTER>=<MY_VALUE_TO_FILTER>
>   - `kubectl get pods -l key=value`

> Get a list of api resources
> kubectl api-resources
> - `kubectl api-resources`

> Change API version of a yaml file
> kubectl convert -f <PATH_OLD_FILE> --output-version <NEW_VERSION>
> - `kubectl convert -f ingress-old.yaml --output-version networking.k8s.io/v1`