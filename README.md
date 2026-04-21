"# My DevOps Project" 



-----
jenkins cmd:

docker volume create jenkins-data

docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins-data:/var/jenkins_home -v //var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts

docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword


docker exec -u root jenkins bash -c "apt-get update && apt-get install -y docker.io"


permission error :
docker exec -u root jenkins chmod 666 /var/run/docker.sock

-----

kubernetes

**Step 1 – Create a deployment**

kubectl create deployment myapp --image=nginx:latest
kubectl get deployments
kubectl get pods

**Step 2 – Scale the deployment**

kubectl scale deployment myapp --replicas=3
kubectl get pods

kubectl scale deployment myapp --replicas=1

**Step 3 – Expose, inspect & clean up**

kubectl expose deployment myapp --port=80 --type=NodePort
kubectl get services
kubectl describe pod <pod-name>
kubectl logs <pod-name>

# Clean up
kubectl delete deployment myapp
kubectl delete service myapp

**Step 4 – Using a YAML manifest (deployment.yaml)**

kubectl apply -f deployment.yaml
kubectl get all

---





