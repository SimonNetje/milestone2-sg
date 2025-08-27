# milestone2-sg
website: http://m2.local:8080/

# Start VM (on host)
```
vagrant up
vagrant ssh

# Start Minikube with 2 nodes
minikube start --nodes=2 --driver=docker

# Enable ingress
minikube addons enable ingress

# Apply all configs (safe to re-run)
kubectl apply -f ~/m2-sg/k8s/00-namespace.yaml
kubectl apply -f ~/m2-sg/k8s/01-secrets-config.yaml
kubectl apply -f ~/m2-sg/k8s/02-postgres.yaml
kubectl apply -f ~/m2-sg/k8s/03-api.yaml
kubectl apply -f ~/m2-sg/k8s/04-web.yaml
kubectl apply -f ~/m2-sg/k8s/05-ingress.yaml

# Check pods
kubectl -n sg get pods -w
```
# shutdown
```
# Stop port-forward (Ctrl+C in terminal where it runs)

# Stop Minikube
minikube stop

# Exit VM
exit

# Stop VM on host
vagrant halt
```
# port forward
```
# Run inside VM, keep terminal open
kubectl -n ingress-nginx port-forward svc/ingress-nginx-controller --address 0.0.0.0 8080:80
```
# change name
```
# Update name in DB via API
curl -i -X POST http://192.168.56.5:8080/api/name \
  -H 'Host: m2.local' \
  -H 'Content-Type: application/json' \
  -d '{"name":"SimonX"}'

# Verify name
curl -s http://192.168.56.5:8080/api/name -H 'Host: m2.local'
```
# plus 2 points
```
# Show nodes
kubectl get nodes -o wide

# Scale API to 4 replicas
kubectl -n sg scale deploy/sg-api --replicas=4

# Check pods spread across nodes
kubectl -n sg get pods -o wide

# Show load balancing (different container IDs on responses)
for i in {1..10}; do curl -s http://m2.local:8080/api/container-id -H 'Host: m2.local'; echo; done
```
