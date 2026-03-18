# kubectl Commands

Quick reference for common kubectl commands.

---

## Configuration & Context

```bash
kubectl config view                                  # Show kubeconfig
kubectl config get-contexts                          # List all contexts
kubectl config current-context                       # Show current context
kubectl config use-context my-cluster                # Switch context
kubectl config set-context --current --namespace=dev # Set default namespace
```

---

## Getting Resources

```bash
kubectl get pods                                     # List pods in current namespace
kubectl get pods -A                                  # All namespaces
kubectl get pods -n staging                          # Specific namespace
kubectl get pods -o wide                             # Extra info (node, IP)
kubectl get pods -o yaml                             # Full YAML output
kubectl get pods -w                                  # Watch for changes
kubectl get pods -l app=web                          # Filter by label
kubectl get all                                      # All resource types
kubectl get nodes                                    # List nodes
kubectl get services                                 # List services
kubectl get deployments                              # List deployments
kubectl get configmaps                               # List configmaps
kubectl get secrets                                  # List secrets
kubectl get ingress                                  # List ingresses
kubectl get pv,pvc                                   # Persistent volumes
kubectl get events --sort-by=.lastTimestamp          # Cluster events
```

---

## Describing Resources

```bash
kubectl describe pod my-pod                          # Detailed pod info & events
kubectl describe node my-node                        # Node info
kubectl describe deployment web-deployment           # Deployment details
```

---

## Creating & Applying Resources

```bash
kubectl apply -f manifest.yaml                       # Apply a YAML file
kubectl apply -f ./manifests/                        # Apply all YAML in a directory
kubectl create deployment web --image=nginx          # Imperative create
kubectl create namespace staging                     # Create a namespace
kubectl create secret generic my-secret --from-literal=key=value
kubectl create configmap my-config --from-file=config.properties
```

---

## Editing Resources

```bash
kubectl edit deployment web-deployment               # Open in editor
kubectl patch deployment web-deployment -p '{"spec":{"replicas":5}}'
kubectl scale deployment web-deployment --replicas=5
kubectl set image deployment/web-deployment web=nginx:1.26
```

---

## Deleting Resources

```bash
kubectl delete pod my-pod                            # Delete a pod
kubectl delete -f manifest.yaml                      # Delete resources defined in file
kubectl delete deployment web-deployment             # Delete a deployment
kubectl delete namespace staging                     # Delete a namespace and all its resources
kubectl delete pod my-pod --grace-period=0 --force   # Force delete (use with care)
```

---

## Debugging & Logs

```bash
kubectl logs my-pod                                  # Container logs
kubectl logs my-pod -c my-container                  # Specific container in a pod
kubectl logs -f my-pod                               # Follow logs
kubectl logs my-pod --previous                       # Logs from previous (crashed) container
kubectl exec -it my-pod -- bash                      # Open shell in a container
kubectl exec -it my-pod -c my-container -- sh        # Shell in specific container
kubectl port-forward pod/my-pod 8080:80              # Forward local port to pod
kubectl port-forward svc/my-service 8080:80          # Forward local port to service
kubectl cp my-pod:/path/to/file ./local-file         # Copy file from pod
kubectl top pods                                     # Resource usage (requires metrics-server)
kubectl top nodes
```

---

## Rollouts

```bash
kubectl rollout status deployment/web-deployment
kubectl rollout history deployment/web-deployment
kubectl rollout undo deployment/web-deployment
kubectl rollout restart deployment/web-deployment    # Rolling restart
```

---

## Quick One-Liners

```bash
# Delete all pods in a namespace (they will restart if managed by a Deployment)
kubectl delete pods --all -n my-namespace

# Get all images running in the cluster
kubectl get pods -A -o jsonpath="{range .items[*]}{.metadata.name}{'\t'}{range .spec.containers[*]}{.image}{'\n'}{end}{end}"

# Watch pod restarts
kubectl get pods -w -o custom-columns=NAME:.metadata.name,RESTARTS:.status.containerStatuses[0].restartCount
```
