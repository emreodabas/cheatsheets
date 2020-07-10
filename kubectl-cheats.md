# kubectl-cheatsheet

## apply resource without file 

cat <<EOF | kubectl apply -f -  //press enter

  $(RESOURCE DEFINITION)         //press enter  
  
EOF                             //press enter    


## port-forward

kubectl port-forward redis-master-765d459796-258hz 6379:6379 


## grant cluster-admin to your current identity
 kubectl create clusterrolebinding emreodabas-cluster-admin-binding --clusterrole=cluster-admin --user=emre.odabas@comodo.com

## drain with grace
kubectl drain --ignore-daemonsets gke-abc-dev-kubernet-pool-4cpu-15gb-283f2af6-9x9j --delete-local-data --grace-period=10 

## get&delete Evicted 

kubectl get po --all-namespaces --field-selector="status.phase!=Running" 

kubectl get po -l type=user-app  --all-namespaces -o json --field-selector="status.phase!=Running" | jq  '.items[]  | "kubectl delete po \(.metadata.name) --n \(.metadata.namespace)"' | xargs -n 1 bash -c

kubectl bulk po --all-namespaces --field-selector="status.phase!=Running" delete


## Check Utilization of Nodes with adding cmd util

alias util='kubectl get nodes | grep node | awk '\''{print $1}'\'' | xargs -I {} sh -c '\''echo {} ; kubectl describe node {} | grep Allocated -A 5 | grep -ve Event -ve Allocated -ve percent -ve -- ; echo '\'''

-> util

## temporary run pod and exec

kubectl run --generator=run-pod/v1 tmp-shell --rm -i --tty --image gcr.io/nucal-development/nettools:1.0.0 -- /bin/bash


## get&delete labeled namespaces and filter created date  

kubectl get ns -l "type"="user-app" -o json | jq -r '.items[] | select(.metadata.creationTimestamp > "'$(date --date="3 day ago" +%F)'") | .metadata.name'

kubectl delete ns $(kubectl get ns -l "type"="user-app" -o json | jq -r '.items[] | select(.metadata.creationTimestamp > "'$(date --date="3 day ago" +%F)'") | .metadata.name')

kubectl get ns -o json | jq -r '.items[] | select(.metadata.creationTimestamp > "'$(date --date="yesterday" +%F)'") | .metadata.name +"    " +.metadata.creationTimestamp'

## find and exec command for pod 

kubectl exec -ti $(kubectl get pods -A -l domainName=abc.com -o jsonpath='{.items[0].metadata.name}{" -n "}{.items[0].metadata.namespace}') -- /bin/sh


## patch service for changing type

kubectl patch svc loki -p '{"spec": {"type": "NodePort"}}'



