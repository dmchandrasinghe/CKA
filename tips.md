- Use the short name of K8s Resources

Short name | Full name

1. cm : configmaps
2. ds : daemonsets
3. deploy deployments
4. ep endpoints
5. ev events
hpa horizontalpodautoscalers
ing ingresses
limits limitranges
ns namespaces
no nodes
pvc persistentvolumeclaims
pv persistentvolumes
po pods
rs replicasets
rc replicationcontrollers
quota resourcequotas
sa serviceaccounts
svc services
Useful commands or parameters during the exam

## Use "kubectl describe" for related events and troubleshooting

kubectl describe pods <podid>

## Use "kubectl explain" to check the structure of a resource object.

kubectl explain deployment --recursive

## Add "-o wide" in order to use wide output, which gives you more details.

kubectl get pods -o wide

## Check always all namespaces by including "--all-namespaces"

kubectl get pods --all-namespaces

## Show labels for all pods (or any other Kubernetes object that supports labelling)

kubectl get pods --show-labels

## create a service

kubectl create service clusterip my-service --tcp=8080 --dry-run=client -o yaml

## create a deployment

kubectl create deployment nginx --image=nginx --dry-run=client -o yaml

## create a pod

kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml
Use dry run to generate yaml
During the exam, creating K8s resources like pods, deployments, and services from scratch can be time-consuming and challenging to remember their entire structure. To simplify this process, you can use the “dry run” feature to generate a basic YAML file. Then, modify the generated file as needed before using it to create the required resources.

For instance, to address the question of creating an nginx pod with specific resource limits (memory: 1M, CPU: 500m), follow these commands:

Generate the YAML file with dry run:

k run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
Modify the “pod.yaml” file to add the resource limit settings.

Create the pod using the modified YAML file:

k create -f pod.yaml
To save time on input, you can define a shell variable for the --dry-run=client -o yaml option like this:

export do="--dry-run=client -o yaml"
Then, you can use the defined variable in the command like this:

k run nginx --image=nginx $do > pod.yaml
By employing the “dry run” and shell variable approach, you can efficiently create K8s resources and manage their configurations during the exam.



Time management
Since you will be executing the kubectl command multiple times, setting up aliases can save you valuable seconds with each entry. For instance, assigning an alias like ‘k’ for ‘kube-control’ can potentially grant you an additional minute or two towards the end of the exam

alias k=kubectl
In the exam, you have the privilege to access and consult the Kubernetes documentation pages for obtaining crucial information. This unique aspect sets the Kubernetes certification exam apart from others, as it assesses your capability to effectively utilize the documentation rather than relying solely on memorization.

To excel in the exam, it is essential to become well-acquainted with the documentation’s structure and practice efficient searching techniques. Please be aware that using bookmarks is not allowed during the exam, so it is advised to refrain from attempting to do so.

During the exam, managing your time efficiently is crucial. With approximately 15 to 20 questions of varying difficulty levels, it’s essential to make strategic decisions regarding time allocation. Don’t get trapped on a single challenging question and exhaust all your time.
