## Deploy on LKE Linode Cluster
- **branch lke_jenkins**
- after creating a cluter on LKE, the kubeconfig.yaml is available
- In the Jenkins it is possible to create credentials as secret file and use this `kubeconfig.yaml` file. 
- to authenticate on LKE cluster, it is necessary to install plugin called `Kubernetes CLI`
- with block `withKubeConfig()` the authentication to LKE can be initiated before submitting *kubectl* command. 