Install Kyverno:
----------------

# Below command will create a namespace "kyverno" and deploy all the components in it.

kubectl create -f https://raw.githubusercontent.com/kyverno/kyverno/main/config/install.yaml



Add the policy to your cluster:
-------------------------------

```
It contains a single validation rule that requires that all Pods have a app.kubernetes.io/name label. Kyverno supports different rule types to validate, mutate, generate, and verify image configurations. The policy attribute validationFailureAction is set to enforce to block API requests that are non-compliant (using the default value audit will report violations but not block requests.)
```

kubectl apply -f kyverno-clusterpolicy.yaml


Try creating a Deployment without the required label:
-----------------------------------------------------

kubectl create deployment nginx --image=nginx


Error reported:
----------------

```
error: failed to create deployment: admission webhook "validate.kyverno.svc-fail" denied the request:

policy Deployment/default/nginx for resource violation:

require-labels:
  autogen-check-for-labels: 'validation error: label ''app.kubernetes.io/name'' is
    required. rule autogen-check-for-labels failed at path /spec/template/metadata/labels/app.kubernetes.io/name/'
```
