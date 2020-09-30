# kube-ops-view-openshift4

How to deploy kube-ops-view to OpenShift 4.x

On a linux host read and implement the following to install helm 3:
https://docs.openshift.com/container-platform/4.5/cli_reference/helm_cli/getting-started-with-helm-on-openshift-container-platform.html#on-linux

Your host will also need git installed.  

Next git clone the helm repo that will contain the kube-ops-view chart:
git clone https://github.com/helm/charts.git

Now change the directory to kube-ops-view
cd charts/stable/kube-ops-view/

Edit the file values.yaml
vi values.yaml

Edit the rbac line 
from-
rbac:
  # If true, create & use RBAC resources
  create: false
  
to-
rbac:
  # If true, create & use RBAC resources
  create: true

Now run lint to make sure all is good with the chart
helm lint

You should receive the following output
$ helm lint
==> Linting .

1 chart(s) linted, 0 chart(s) failed

Now you must run the following to download the redis dependancy for this project.
helm dependency update

Now move up one directory.
cd ..

Next you need to create your OpenShift project
oc new-project ops-view

Now you can install kube-ops-view to your OpenShift 4.x cluster
helm install kube-ops-view --generate-name

Next you need to find your service that is created for kube-ops-view
oc get svc
NAME                       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S) 
kube-ops-view-1601426497   ClusterIP   172.30.252.168   <none>        80/TCP    
  
Now expose the service so you can reach kube-ops-view external to the cluster
oc create route edge --service=kube-ops-view-1601426497
