## Complete CICD pipeline with DockerHub and EKS
**branch jenkins_dockerhub**
- template files inside the kubernetes directory are created for deployment and service components
- in the Jenkinsfile the environmental variables for application name, image name and repository name are defined. 
- values of environmental variables are passed to `deployment.yaml` and `service.yaml` by using a tool `envsubst`. This tool is installed inside the jenkins container using a command: <br>
*apt-get install gettext-base* 
- EKS cluster is authenticated on the DockerHub repository using a secret component: <br>
    *docker create secret docker-registry my-registry-key --docker-server=docker.io --docker-username=username --docker-password=password*
- secret component is referenced inside the `deployment.yaml` by an attribute `imagePullSecrets`