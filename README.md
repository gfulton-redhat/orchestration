# openshift-gitops



## Install Steps
1. Login to openshift.
    * `oc login --token=<YOUR TOKEN> --server=<YOUR SERVER>` 
2. Install the OpenShift GitOps Operator and grant it RBAC permissions to install the remaining resources.
    * `kustomize build bootstrap | oc apply -f-`
3. Wait for the operator to start Red Hat OpenShift GitOps. 
5. Install applications using ArgoCD.
    * `kustomize build applications | oc apply -f-`
6. Load secrets into vault by executing the following commands.
```bash
oc exec -n vault -it vault-0 -- vault kv put \
   secret/github \
   username=<your-github-username> \
   password=<your-github-pat>
   
oc exec -n vault -it vault-0 -- vault kv put \
   secret/registry/redhat \
   host=registry.access.redhat.com \
   
   user=<your-username> password=<your-password>
oc exec -n vault -it vault-0 -- vault kv put \
   secret/registry/quay \
   host=quay.io \
   user=<your-username> \
   password=<your-password>
   
oc exec -n vault -it vault-0 -- vault kv put \
   secret/argocd \
   username=<your-username> \
   password=<your-password>
```