# Instructions for Upgrading Prometheus

To upgrade prometheus, follow the following steps. Follow the correct steps for your infrastrucutral setup.

## 1. **Delete your  old prometheus installation**

Run the following commands to remove the exisitng Prometheus installation:
```
kubectl delete -f promethus_to_delete.yaml
kubectl -n cnvrg  get pvc | grep prometheus | awk '{print $1}' | xargs kubectl -n cnvrg delete pvc $1
kubectl delete --ignore-not-found=true -f manifests/ -f manifests/setup
```
## **For installation on Hostpath Directory with Istio**
- Change nodeaffinity in `manifests/prometheus-prometheus.yaml` file
- Make sure the local path is correct 
- Change hosts to correct domain_name in prometheus-vs.yaml file
```
rm manifests/prometheus-prometheus_cloud.yml
```
#### **For installation on instance using default storageclass other then hostpath - Installed with Istio**
- Change hosts to correct domain_name in prometheus-vs.yaml file
```
rm manifests/prometheus-prometheus.yml
```

#### **Install Prometheus**
```
kubectl apply -f manifests/setup
kubectl apply -f manifests/
until kubectl get servicemonitors --all-namespaces ; do date; sleep 1; echo ""; done
```


#### clean resources
```
kubectl delete --ignore-not-found=true -f manifests/ -f manifests/setup
```
