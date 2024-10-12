minikube start --cpus=6 --memory 7838 --disk-size 32g


kubectl port-forward service/my-grafana 3000:80

skaffold dev 