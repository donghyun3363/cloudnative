
# Kubernetes

kubectl run hello-world
kubectl delete po hello-world
kubectl delete po -all

kubectl get po --show-labels
kubectl get po -l app,mode
kubectl get po -l app=account
kubectl get po -l '!mode'

kubectl label po hello-world app=ui
kubectl label po hello-world app=ui --overwrite
kubectl label node oke-hellow-123-481274-hokf gpu=true
kubectl get nodes -l gpu=true

kubectl create -f hello-world-gpu.yaml
kubectl delete po -l mode=dev
kubectl get ns
kubectl get po --namespace myns
kubectl get po -n myns

kubectl create namespace myns
kubectl delete ns myns
kubectl create -f hello-world.yaml -n test-namespace
kubectl get po -n test
kubectl delete all --all
kubectl delete all --all -n myns

kubectl create -f myrc.yaml
kubectl get rc
kubectl edit rc myrc
kubectl scale rc myrc --replicas=10
kubectl delete rc myrc
kubectl delete rc myrc --cascade=false


kubectl get rs

kubectl create -f myservice.yaml
kubectl get svc
kubectl get endpoints myservice
kubectl delete svc myservice

