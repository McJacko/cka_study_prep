| Question | Answer |
|----------|--------|
| How do you create a Kubernetes cluster? | Use tools like `kubeadm`, `kops`, or managed services like EKS, AKS, GKE. |
| How do you deploy a pod using a YAML file? | Use `kubectl apply -f <file.yaml>` to create or update resources from a YAML file. |
| How do you scale a deployment? | Use `kubectl scale deployment <deployment-name> --replicas=<number>` to scale a deployment to the desired number of replicas. |
| How do you check the status of a node? | Use `kubectl get nodes` to list nodes and `kubectl describe node <node-name>` for detailed status. |
| How do you set resource requests and limits for a pod? | Define `resources.requests` and `resources.limits` in the pod's YAML specification. |
| How do you expose a deployment as a service? | Use `kubectl expose deployment <deployment-name> --type=<type> --port=<port>` to create a service. |
| How do you troubleshoot a failing pod? | Use `kubectl describe pod <pod-name>` and `kubectl logs <pod-name>` to gather information about the pod. |
| How do you create a ConfigMap? | Use `kubectl create configmap <configmap-name> --from-literal=<key>=<value>` or from a file with `--from-file=<path>`. |
| How do you create a Secret? | Use `kubectl create secret generic <secret-name> --from-literal=<key>=<value>` or from a file with `--from-file=<path>`. |
| How do you perform a rolling update on a deployment? | Update the deployment configuration and use `kubectl apply -f <file.yaml>`. Monitor with `kubectl rollout status deployment <deployment-name>`. |
| How do you rollback a deployment? | Use `kubectl rollout undo deployment <deployment-name>` to revert to the previous version. |
| How do you configure a pod to use a ConfigMap? | Mount the ConfigMap as a volume or use environment variables in the pod specification. |
| How do you configure a pod to use a Secret? | Mount the Secret as a volume or use environment variables in the pod specification. |
| How do you create a namespace? | Use `kubectl create namespace <namespace-name>` to create a new namespace. |
| How do you switch between namespaces? | Use `kubectl config set-context --current --namespace=<namespace-name>` to set the default namespace for the current context. |
| How do you restrict access to a namespace? | Use Role-Based Access Control (RBAC) to create roles and role bindings. |
| How do you create a PersistentVolume (PV) and PersistentVolumeClaim (PVC)? | Define and apply PV and PVC YAML files using `kubectl apply -f <file.yaml>`. |
| How do you configure network policies? | Use NetworkPolicy resources to define rules for network communication between pods. |
| How do you use Helm to deploy applications? | Install Helm, add repositories, and use `helm install <release-name> <chart>` to deploy applications. |
| How do you set up a Kubernetes cluster with high availability (HA)? | Use multiple control plane nodes and etcd members. Configure load balancing and failover mechanisms. |
| How do you back up and restore etcd? | Use `etcdctl snapshot save <backup-file>` for backup and `etcdctl snapshot restore <backup-file>` for restore. |
| How do you secure the Kubernetes API server? | Use authentication, authorization, and admission control. Secure communication with TLS. |
| How do you monitor a Kubernetes cluster? | Use tools like Prometheus, Grafana, and Kubernetes metrics server for monitoring. |
| How do you set up logging in Kubernetes? | Use tools like Elasticsearch, Fluentd, and Kibana (EFK) or Fluent Bit for centralized logging. |
| How do you upgrade a Kubernetes cluster? | Follow the official Kubernetes upgrade documentation. Use tools like `kubeadm upgrade` for managed upgrades. |
| How do you configure ingress for a Kubernetes application? | Use Ingress resources and controllers to manage external access to services in a cluster. |
| How do you manage Kubernetes certificates? | Use kubeadm, cert-manager, or manually manage certificates for control plane components. |
| How do you configure pod affinity and anti-affinity? | Define `affinity` rules in the pod specification to control pod placement based on labels. |
| How do you use taints and tolerations? | Apply taints to nodes with `kubectl taint nodes <node-name> <key>=<value>:<effect>`. Pods use tolerations to schedule onto tainted nodes. |
| How do you create a custom scheduler? | Write and deploy a custom scheduler. Configure pods to use the custom scheduler by specifying it in the pod's YAML file. |
