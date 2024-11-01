minikube start --cpus=6 --memory 7838 --disk-size 32g


kubectl port-forward service/my-grafana 3000:80

http://tempo-query-frontend:3100
http://loki:3100

skaffold dev 