| Top Kubectl Commands | Explanation |
|---------|-------------|
| `kubectl get pods` | Lists all pods in the current namespace. Add `-n <namespace>` to specify a different namespace. |
| `kubectl get nodes` | Lists all nodes in the Kubernetes cluster. |
| `kubectl get services` | Lists all services in the current namespace. Add `-n <namespace>` to specify a different namespace. |
| `kubectl get deployments` | Lists all deployments in the current namespace. Add `-n <namespace>` to specify a different namespace. |
| `kubectl get namespaces` | Lists all namespaces in the Kubernetes cluster. |
| `kubectl describe pod <pod-name>` | Provides detailed information about a specific pod, including events and resource usage. |
| `kubectl describe node <node-name>` | Provides detailed information about a specific node, including its status, capacity, and allocated resources. |
| `kubectl describe service <service-name>` | Provides detailed information about a specific service. |
| `kubectl describe deployment <deployment-name>` | Provides detailed information about a specific deployment, including its strategy, replicas, and selector. |
| `kubectl logs <pod-name>` | Retrieves logs for a specific pod. Use `-c <container-name>` to specify a container within the pod. |
| `kubectl exec <pod-name> -- <command>` | Executes a command inside a container in a pod. Use `-c <container-name>` to specify a container. |
| `kubectl apply -f <file.yaml>` | Applies a configuration change from a YAML file to create or update resources. |
| `kubectl create -f <file.yaml>` | Creates resources from a YAML file. |
| `kubectl delete pod <pod-name>` | Deletes a specific pod. |
| `kubectl delete service <service-name>` | Deletes a specific service. |
| `kubectl delete deployment <deployment-name>` | Deletes a specific deployment. |
| `kubectl scale deployment <deployment-name> --replicas=<number>` | Scales a deployment to a specified number of replicas. |
| `kubectl edit deployment <deployment-name>` | Opens an editor to modify the configuration of a specific deployment. |
| `kubectl rollout status deployment <deployment-name>` | Checks the status of a deployment rollout. |
| `kubectl rollout undo deployment <deployment-name>` | Rolls back a deployment to a previous revision. |
| `kubectl port-forward <pod-name> <local-port>:<pod-port>` | Forwards a local port to a port on a pod, enabling access to the pod’s applications from your local machine. |
| `kubectl top pods` | Displays resource usage (CPU and memory) for pods. |
| `kubectl top nodes` | Displays resource usage (CPU and memory) for nodes. |
| `kubectl get configmap <configmap-name>` | Retrieves information about a specific ConfigMap. |
| `kubectl get secret <secret-name>` | Retrieves information about a specific Secret. |
| `kubectl create namespace <namespace-name>` | Creates a new namespace. |
| `kubectl delete namespace <namespace-name>` | Deletes a specific namespace and all its resources. |
| `kubectl apply -k <directory>` | Applies a set of Kustomize configurations from a directory. |
| `kubectl cp <source> <pod-name>:<destination>` | Copies files or directories to and from containers in a pod. |
| `kubectl patch <resource> <name> --patch <patch-data>` | Updates a resource using a strategic merge patch, JSON merge patch, or JSON patch. |
| `kubectl get pods -o wide` | Lists all pods with additional information such as node placement and IP addresses. |
| `kubectl get events --sort-by=.metadata.creationTimestamp` | Lists all events in the cluster, sorted by creation timestamp. |
| `kubectl get all --all-namespaces` | Lists all resources (pods, services, deployments, etc.) in all namespaces. |
| `kubectl explain <resource>` | Provides documentation on a specific resource type, including its fields and usage. |
| `kubectl apply -f <file.yaml> --record` | Applies a configuration change from a YAML file and records the change in the resource's annotations. |
| `kubectl set image deployment/<deployment-name> <container-name>=<image>` | Updates the image of a container in a deployment. |
| `kubectl autoscale deployment <deployment-name> --min=<min-replicas> --max=<max-replicas> --cpu-percent=<target-cpu-usage>` | Sets up autoscaling for a deployment based on CPU usage. |
| `kubectl run <pod-name> --image=<image> --port=<port> --env=<key>=<value>` | Creates and runs a pod with the specified image, port, and environment variables. |
| `kubectl annotate pod <pod-name> <annotation-key>=<annotation-value>` | Adds or updates an annotation on a pod. |
| `kubectl label pod <pod-name> <label-key>=<label-value>` | Adds or updates a label on a pod. |
| `kubectl taint nodes <node-name> <key>=<value>:<effect>` | Applies a taint to a node to control which pods can be scheduled on it. |
| `kubectl cordon <node-name>` | Marks a node as unschedulable, preventing new pods from being scheduled on it. |
| `kubectl drain <node-name> --ignore-daemonsets` | Evicts all pods from a node, preparing it for maintenance. |
| `kubectl get pod <pod-name> -o jsonpath='{.status.podIP}'` | Retrieves the IP address of a specific pod using a JSONPath expression. |
| `kubectl get deployment <deployment-name> -o yaml > <file.yaml>` | Exports the YAML configuration of a deployment to a file. |
| `kubectl port-forward svc/<service-name> <local-port>:<service-port>` | Forwards a local port to a port on a service, enabling access to the service from your local machine. |
| `kubectl patch deployment <deployment-name> --patch '{"spec":{"template":{"spec":{"containers":[{"name":"<container-name>","image":"<new-image>"}]}}}}'` | Updates the image of a container in a deployment using a JSON patch. |
| `kubectl create configmap <configmap-name> --from-file=<file-path>` | Creates a ConfigMap from a file or directory. |
| `kubectl create secret generic <secret-name> --from-literal=<key>=<value>` | Creates a generic secret from literal values. |
| `kubectl rollout history deployment/<deployment-name>` | Displays the rollout history of a deployment. |
