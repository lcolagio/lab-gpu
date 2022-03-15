# lab-gpu

## NVIDIA GPU

https://docs.nvidia.com/datacenter/cloud-native/gpu-operator/openshift/contents.html

## Installation on OCP 4.7 / NVidia GPU 1.8


## Installation on OCP 4.8 / NVidia GPU 1.9

```
apiVersion: v1
kind: Namespace
metadata:
  name: nvidia-gpu-operator
```

```
oc label ns/$NAMESPACE_NAME openshift.io/cluster-monitoring=true
```

```
apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
  name: nvidia-gpu-operator-group
  namespace: nvidia-gpu-operator
spec:
 targetNamespaces:
 - nvidia-gpu-operator
```

```
oc get packagemanifest gpu-operator-certified -n openshift-marketplace -o jsonpath='{.status.defaultChannel}'
```

CHANNEL=v1.9.0

```
oc get packagemanifests/gpu-operator-certified -n openshift-marketplace -ojson | jq -r '.status.channels[] | select(.name == "'$CHANNEL'") | .currentCSV'
```

```
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: gpu-operator-certified
  namespace: nvidia-gpu-operator
spec:
  channel: "v1.9.0"
  installPlanApproval: Manual
  name: gpu-operator-certified
  source: certified-operators
  sourceNamespace: openshift-marketplace
  startingCSV: "gpu-operator-certified.v1.9.0"
```  

```
oc get installplan -n nvidia-gpu-operator
INSTALL_PLAN=$(oc get installplan -n nvidia-gpu-operator -oname)

oc patch $INSTALL_PLAN -n nvidia-gpu-operator --type merge --patch '{"spec":{"approved":true }}'
```



