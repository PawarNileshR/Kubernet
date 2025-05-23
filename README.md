🚀 **Deploy WordPress on Kubernetes – Comprehensive Step-by-Step Guide**

📁 **Pre-requisite: Navigate into your WordPress Kubernetes Project Directory**

Before you begin, navigate into the directory where your WordPress deployment YAML files and configurations are located:

cd /path/to/your/wordpress-k8s/

🧱 **Step 1: Create a Dedicated Namespace**

This namespace isolates your WordPress environment from other workloads:

kubectl apply -f namespace.yaml

🔐 Step 2: **Create a Kubernetes Secret (for MariaDB credentials)**

This secret securely stores the database username and password:

kubectl apply -f secrets.yaml

⚙️ Step 3: **Create a ConfigMap (for MariaDB Environment Variables)**

Optional but useful for managing database environment configurations like DB name and user:

kubectl apply -f configmap.yaml

💾 **Step 4: Setup EBS Storage Class (for AWS EBS CSI Driver)**

Ensure that your EBS CSI driver is configured before proceeding. This storage class allows persistent volume provisioning:

cd aws-ebs-csi-driver/deploy/kubernetes/overlays/stable
kubectl apply -k .
cd -
kubectl apply -f ebs-sc.yaml

🔀 Ensure your Kubernetes cluster is running on AWS and that IAM roles and CSI driver installation are completed successfully.

🐬 **Step 5: Deploy the MariaDB Stateful Database**

This deployment manages the MariaDB database required by WordPress:

kubectl apply -f mariadb-deployment.yaml

🌐 **Step 6: Deploy the WordPress Application**

This sets up the WordPress frontend along with its pod specifications:

kubectl apply -f wordpres-deployment.yaml

🔗 **Step 7: Expose WordPress and MariaDB with Kubernetes Services**

These services make your WordPress frontend and database reachable inside the cluster:

kubectl apply -f wordpresMariadb-service.yaml

🌍 **Step 8: Deploy Ingress Controller using NodePort**

The ingress controller is used to route external traffic to the services. NodePort exposes it for testing:

kubectl apply -f ingress-nginx-service-nodeport.yaml

🚪 **Step 9: Deploy the Ingress Resource for Routing**

This defines routing rules for accessing WordPress via a domain name:

kubectl apply -f ingress.yaml

🔀 Ensure the IngressClassName in your ingress.yaml file matches the controller you installed (commonly nginx).

✅ Validate Your WordPress Deployment

⏱ **List All Resources in the Namespace**

Check if all the components like pods, services, and deployments are active:

kubectl get all -n wordpress-prod

🌐 **Verify the Ingress Resource and its IP Address**

Ensure the ingress resource is created correctly and check its external IP:

kubectl get ingress -n wordpress-prod

🌐 **Accessing WordPress Interface**

🖥️ If Using NodePort:

Open the WordPress site using your node’s public IP and NodePort:

http://<NODE_PUBLIC_IP>:<NODE_PORT>

🌐 **If Using Ingress and a Custom Hostname:**

Update your local machine’s hosts file:

<node-public-ip> wordpress.local

Visit the site in your browser:

http://wordpress.local


########################################################################################################################


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






