
## Deploy to EKS Cluster from Jenkins Pipeline
- **branch eks_jenkins**
- install kubectl tool inside Jenkins container on Jenkins server: <br>
*curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl; chmod +x ./kubectl; mv ./kubectl /usr/local/bin/kubectl*
- install aws iam authenticator inside Jenkins container on Jenkins server: <br>
*curl -Lo aws-iam-authenticator https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v0.6.11/aws-iam-authenticator_0.6.11_linux_amd64* <br>
*chmod +x ./aws-iam-authenticator*<br>
*mv ./aws-iam-authenticator /usr/local/bin* <br>

- kubeconfig file must be available in .kube directory inside jenkins home directory of the container: <br>
Execute command from jenkins server: *docker cp config 174985fe61a0:/var/jenkins_home/.kube*

- AWS_ACCESS_KEY_ID, AWS_SECRET_ACCES