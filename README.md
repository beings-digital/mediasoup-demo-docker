## Auth
```
gcloud auth login
gcloud config set project beam-372120
gcloud container clusters get-credentials beam-test-stunner-cluster-1 --zone asia-southeast1-a --project beam-372120

```

## Deploy Stunner
### Build image
```
cd server
gcloud auth configure-docker asia-southeast1-docker.pkg.dev
export GOOGLE_APPLICATION_CREDENTIALS="./credentials.json"
docker build . -t mediasoup-demo-docker
docker tag mediasoup-demo-docker:latest asia-southeast1-docker.pkg.dev/beam-372120/mediasoup-demo/mediasoup-demo-docker:latest
docker push asia-southeast1-docker.pkg.dev/beam-372120/mediasoup-demo/mediasoup-demo-docker:latest
```

### Deploy stunner gateway
```
helm repo add stunner https://l7mp.io/stunner
helm repo update
helm install stunner-gateway-operator stunner/stunner-gateway-operator-dev --create-namespace --namespace=stunner-system

kubectl apply -f mediasoup-call-stunner.yaml

```

### Deploy services
```

kubectl apply -n mediasoup -f mediasoup-server.yaml
kubectl apply -n mediasoup -f mediasoup-server-services.yaml 
kubectl apply -f letsencrypt-cluster-issuer.yaml
kubectl apply -n mediasoup -f mediasoup-ingress.yaml
```



