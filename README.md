##########  **Deploy Wordpress On Kubernet**  ############

#################  **Step-by-Step**  #####################

**Go inside the WordPress directory and run the command step by step**

**Create Namespace**

kubectl apply -f namespace.yaml

**Create Secret (DB Password etc.)**

kubectl apply -f secrets.yaml

**Create ConfigMap (Optional but used for DB env config)**

kubectl apply -f configmap.yaml

**Create EBS Storage Class (if you're using EBS)**

cd aws-ebs-csi-driver/deploy/kubernetes/overlays/stable
kubectl apply -k .
kubectl apply -f ebs-sc.yaml

**Deploy MariaDB**

kubectl apply -f mariadb-deployment.yaml

**Deploy WordPress**

kubectl apply -f wordpres-deployment.yaml

**Service WordPressANDMariaDB**

kubectl apply -f wordpresMariadb-service.yaml

**Expose WordPress with a NodePort (if you're using it)**

kubectl apply -f ingress-nginx-service-nodeport.yaml

**Deploy Ingress**

kubectl apply -f ingress.yaml


###########  **Verify Everything**  ##############

**List all resources**
kubectl get all -n wordpress-prod
kubectl get ingress -n wordpress-prod

**Check Ingress IP**
kubectl get ingress -n wordpress-prod

################  **Access via browser**  #####################

http://<node-public-ip>:<nodeport> 


