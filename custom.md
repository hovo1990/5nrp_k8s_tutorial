
mkdir -p ~/.kube/  
cp config ~/.kube/config 




kubectl create -f simple.yaml
kubectl get pods -o wide

kubectl exec -it pod-hgrabski -- /bin/bash


# AI Training

kubectl logs gp3-username
kubectl apply -f pytorch-training.yaml

kubectl delete -f gp3-hgrabski