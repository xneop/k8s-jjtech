# k8s-jjtech

## Deploy EKS CLuster with Fargate using Terraform

### Prerequisites:
* Terraform installed
* AWS Account and AWS cli installed
* Kubectl installed. Follow this url for installation guide **https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html**

### Note

* This cluster based on the current configuration will be created in the us-east-1 regiona and is setup to wotk only with fargate
* Rerun terraform apply again if you receive **Error: creating EC2 VPC: RequestError: send request failed
│ caused by: Post "https://ec2.us-east-1.amazonaws.com/": read tcp 192.168.1.71:53727->209.54.181.193:443: wsarecv: An existing connection was forcibly closed by the remote host.** and it should deploy the resources
* The deployment takes over 15 minutes

### Resources Created
* VPC and all networking components
* EKS cluster
* aws load balancer controller for ingress
* Fargate profiles for application, aws-load-balancer-controller  and Kube-system namespaces

### Deployment Steps

1. terraform init
2. terraform fmt
3. terraform validate
4. terraform plan
5. terraform apply --auto-appprove

### steps after succesful deployment of cluster with terraform 

* After you provision the EKS with terraform, you would need to update your Kubernetes context to access the cluster with the following command - **aws eks update-kubeconfig --name name-of-cluster --region region-where-cluster-is-deployed**

* To patch coredns to run on fargate nodes, run the command below 

**kubectl patch deployment coredns \
-n kube-system \
--type json \
-p='[{"op": "remove", "path": "/spec/template/metadata/annotations/eks.amazonaws.com~1compute-type"}]'**
