Infosys - 9thJuly Session

https://developer.ibm.com/tutorials/developing-a-kubernetes-application-with-local-and-remote-clusters/
https://github.com/IBMDevConnect/helm101labs


- Create a kubernetes cluster on IBM Cloud
- Once the cluster is normal
- ibmcloud login
- Need to export the KUBECONFIG environment path, so that we can talk to the IBM Cloud kubernetes cluster from command line
- ibmcloud ks cluster-config mycluster
- export the KUBECONFIG value
- Check if the kubectl command line is configured… “kubectl get nodes”
- Build the docker image of the application we are working “docker build -t guestbook:v1 .”
- Check if the docker image for the guestbook has been created “docker images”
- Now lets push this image onto ibmcloud registry 
- Check which region you are targeting “ibmcloud cr region”
- Now lets take our local docker image “docker tag guestbook:v1.0 registry.<region>.bluemix.net/<my_namespace>/guestbook:v1.1”
- Now push the image to ibm cloud repository “docker push registry.ng.bluemix.net/vthink/guestbook:v1”
- Now create the deployment “kubectl run guestbook --image=registry.ng.bluemix.net/vthink/guestbook:v1”
- Verify of we have the guestbook pod running “kubectl get pods”
- Now access the running application “kubectl expose deployment guestbook --type=NodePort --port=3000”
- In order to access this application, we need the public IP address “ibmcloud ks workers mycluster”
- Now get the application port that is mapped “kubectl describe services/guestbook”
- Access the application from browser… You need to see the guestbook landing page



Helm: Install the guestbook application as helm chart
- git clone https://github.com/IBMDevConnect/helm101
- Initialize helm “helm init” and “helm version”
- Lets prepare to install the guestbook application as helm chart
- $ kubectl create serviceaccount --namespace kube-system tiller   (A service account provides an identity for processes that run in a Pod.)
- $ kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
- $ kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
- $ cd helm101/charts
- $ helm install ./guestbook/ --name guestbook-demo --namespace helm-demo
- Check the if the installation is complete “kubectl get svc -w guestbook-demo --namespace helm-demo”

helm search mongo  - To search for a helm chart from the existing repo
helm template .  - To check how the templating engine updates the values, and submits to the tiller service


kubectl create –f tomcat.yml
