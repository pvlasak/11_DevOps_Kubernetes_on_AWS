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