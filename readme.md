minikube vpc 5173 default:

forwarded to 8080
adity@Adis-MacBook cosmocloud % kubectl port-forward frontend-deployment-9c98c6858-vwr2n 8080:5173
Forwarding from 127.0.0.1:8080 -> 5173
Forwarding from 8080 -> 5173
Handling connection for 8080
Handling connection for 8080


minikube start 
helm command -dry run
helm install testapp cosmocloud-deploy --atomic --timeout 30s
kubectl port-forward frontend-deployment-9c98c6858-vwr2n 8080:5173   
minikube service frontend-svc 
^C%                                                                                                                                                      
adity@Adis-MacBook cosmocloud % kubectl port-forward frontend-deployment-9c98c6858-vwr2n 31000:5173 
Forwarding from 127.0.0.1:31000 -> 5173
Forwarding from [::1]:31000 -> 5173
Handling connection for 31000
Handling connection for 31000
