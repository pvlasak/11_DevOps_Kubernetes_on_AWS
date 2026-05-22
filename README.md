# 11_DevOps_Kubernetes_on_AWS
this repository demonstrates running Kubernetes cluster on AWS, Autoscaling 

## EKS Cluster in AWS
- On AWS an EKS cluster has to be created, cluster has a role to manage instances running inside my AWS account.
- Control plane of EKS cluster is running outside my AWS account.
- VPC has to be created, together with subnets (private and public) and security groups. 
- EKS cluster is running in this VPC 
- worker nodes runnig as EC2 instances are managed by a `node group`, node group makes sure that kubelet, kube-proxy and docker container runtime are configured on EC2 instances.
- Worker nodes are running in a private subnet and will get an IP address from the subnet range. 

## Autoscaling of worker nodes
- EKS Cluster has OIDC URL automatically assigned. 
- EKS Cluster is running outside my AWS account has to get a permission to create or delete EC2 instances to scale them up or down. 
- It is neccessary to create an OIDC trust between EKS cluster and Identity Provider to allow EKS cluster to assume a web-identity role to scale EC2 instances.
- On the EKS cluster a auto-scaler component is deployed using a yaml file: `cluster_autoscaler.yaml`
- Policy assigned to a role that auto-scaler may assume is configured in the JSON file `policy_autoscale_ec2.json` and attached to a role.
- Auto-Scaler component communicates with auto-scaling group that is assigned to a node group collecting the EC2 instances, tags assigned to an auto-scaling group are used to identify an EKS cluster that is allowed to communicate with auto-scaling group. 

## Deploy to EKS Cluster from Jenkins Pipeline
- install kubectl tool inside Jenkins container on Jenkins server: <br>
*curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl; chmod +x ./kubectl; mv ./kubectl /usr/local/bin/kubectl*
- install aws iam authenticator inside Jenkins container on Jenkins server: <br>
*curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.6.11/aws-iam-authenticator_0.6.11_linux_amd64* <br>
*chmod +x ./aws-iam-authenticator*<br>
*mv ./aws-iam-authenticator /usr/local/bin* <br>

- kubeconfig file must be available in .kube directory inside jenkins home directory of the container: <br>
Execute command from jenkins server: *docker cp config 174985fe61a0:/var/jenkins_home/.kube*

- AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY defined as environmental variables in the Jenkinsfile. Jenkins holds credentials as secret text. 


