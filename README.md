# helm-sample
```
oc new-project tiller
export TILLER_NAMESPACE=tiller
curl -s https://get.helm.sh/helm-v2.15.2-linux-amd64.tar.gz
oc process -f tiller-template.yaml -p TILLER_NAMESPACE="${TILLER_NAMESPACE}" -oyaml | oc create -f -

TILLER_SAMPLEAPP_NAMESPACE=tiller-sampleapp-project
oc new-project ${TILLER_SAMPLEAPP_NAMESPACE}
oc policy add-role-to-user edit "system:serviceaccount:${TILLER_NAMESPACE}:tiller"
```
