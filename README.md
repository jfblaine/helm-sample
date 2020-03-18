# helm-sample
```
# helm install and helpful commands
wget https://get.helm.sh/helm-v2.16.3-linux-amd64.tar.gz
tar xvf helm-v2.16.3-linux-amd64.tar.gz
./linux-amd64/helm version
helm list --all
helm del <release> --purge # to permanently delete
helm install <tar|dir|etc> # --debug
helm install --dry-run --debug ./nodejs/
# override/set a value listed in values.yaml, 'application_domain' in this case:
helm install --dry-run --debug ./nodejs/ --set application_domain=nodejs.apps.home.io

# connect to different tillers
# environment setup
export TILLER_NAMESPACE=tiller-1; export TILLER_APP_NAMESPACE=tiller-app-1
oc process -f tiller-template.yaml -oyaml -p TILLER_NAMESPACE=${TILLER_NAMESPACE} -p TILLER_APP_NAMESPACE=${TILLER_APP_NAMESPACE} | oc create -f -
oc policy add-role-to-user edit "system:serviceaccount:${TILLER_NAMESPACE}:tiller" -n ${TILLER_APP_NAMESPACE}

export TILLER_NAMESPACE=tiller-2; export TILLER_APP_NAMESPACE=tiller-app-2
oc process -f tiller-template.yaml -oyaml -p TILLER_NAMESPACE=${TILLER_NAMESPACE} -p TILLER_APP_NAMESPACE=${TILLER_APP_NAMESPACE} | oc create -f -
oc policy add-role-to-user edit "system:serviceaccount:${TILLER_NAMESPACE}:tiller" -n ${TILLER_APP_NAMESPACE}

# deploy to multiple tillers/namespaces from one helm
export TILLER_NAMESPACE=tiller-1; export TILLER_APP_NAMESPACE=tiller-app-1; export APP_NAME=node-1
helm init --service-account tiller --tiller-namespace ${TILLER_NAMESPACE} --client-only
helm install -n ${APP_NAME} ./nodejs/ --tiller-namespace ${TILLER_NAMESPACE} --namespace ${TILLER_APP_NAMESPACE}
helm list --all
# NAME    REVISION        UPDATED                         STATUS          CHART           APP VERSION     NAMESPACE
# node-1  1               Fri Nov  1 21:09:20 2019        DEPLOYED        nodejs-0.1                      tiller-app-1
# curl -s <app1-route-hostname> | grep "Node.js application on OpenShift"
#             <h1>Welcome to your Node.js application on OpenShift</h1>


export TILLER_NAMESPACE=tiller-2; export TILLER_APP_NAMESPACE=tiller-app-2; export APP_NAME=node-2
helm init --service-account tiller --tiller-namespace ${TILLER_NAMESPACE} --client-only
helm install -n ${APP_NAME} ./nodejs/ --tiller-namespace ${TILLER_NAMESPACE} --namespace ${TILLER_APP_NAMESPACE}
helm list --all
# NAME    REVISION        UPDATED                         STATUS          CHART           APP VERSION     NAMESPACE
# node-2  1               Fri Nov  1 21:09:59 2019        DEPLOYED        nodejs-0.1                      tiller-app-2
# curl -s <app2-route-hostname> | grep "Node.js application on OpenShift"
#             <h1>Welcome to your Node.js application on OpenShift</h1>
```

