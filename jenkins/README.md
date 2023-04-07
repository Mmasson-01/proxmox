# jenkins

## Add Helm repo
helm repo add jenkins https://charts.jenkins.io

## Create PV
kubectl apply -f pv.yaml

## Install helm charts
helm install jenkins -n jenkins -f values.yaml jenkins/jenkins
