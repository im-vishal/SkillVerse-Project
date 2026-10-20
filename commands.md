>## Steps to deploy the project locally on KIND cluster

### 1. Create Cluster & deploy images

```bash
docker compose up --build

kind create cluster \
  --config k8s/kind-config.yaml \
  --name skillverse

kind load docker-image \
  virtualvishal/skillverse-backend:latest \
  --name skillverse

kind load docker-image \
  virtualvishal/skillverse-frontend:latest \
  --name skillverse

# to see the images in kind control plane
docker exec -it skillverse-control-plane crictl images

# info
kubectl cluster-info
kind get clusters
kubectl get nodes
kubectl get pods -n skillverse
kubectl logs -f $PODNAME -n skillverse
kubectl describe pod $PODNAME -n skillverse

# Show me the container images that this Kubernetes node's container runtime knows about
docker exec -it skillverse-control-plane crictl images

kubectl apply \
  -f k8s/00-namespace.yaml \
  -f k8s/10-mysql.yaml \
  -f k8s/20-backend.yaml \
  -f k8s/30-frontend.yaml

kubectl rollout status statefulset/mysql \
  -n skillverse \
  --timeout=180s

kubectl rollout status deployment/backend \
  -n skillverse \
  --timeout=120s

kubectl rollout status deployment/frontend \
  -n skillverse \
  --timeout=60s

echo
echo "skillverse is live at http://localhost:8888"
echo

kind delete cluster --name $CLUSTERNAME
```

### 2. Add Taskfile for one command deployment
```taskfile
- task up
- task down
```

Refer `kind-local-taskfile.yml`

>## Steps to deploy the project on eks

## 1. Edit `30-frontend.yml` file
- Change the service.spec.type to `LoadBalancer` & remove service.spec.ports.nodePort

## 2. Edit `10-mysql.yml` file
- Change VolumeClaimTemplates in statefulset in 

## 3. Create `05-storage-class.yml` file

## 4. Add ebs-csi.tf & modify eks.tf accordingly

## 5. Add Taskfile



