# Metabase on OpenShift

Openshift Template for Metabase backed by PostgreSQL

#### Create Template
```sh
oc create -f metabase.yml
```

#### Delete Instance Resources
Clean up all resources created. Note label filters assume single instance of template deployed in the current namespace.

```sh
oc delete all -l app=metabase
oc delete pods -l app=metabase
oc delete persistentvolumeclaim -l app=metabase
oc delete serviceaccount -l app=metabase
oc delete secret -l app=metabase
```

#### Gotchas
##### Resource Limits
When deploying this template it is important to note that if pod limits are set and default container limits match pod limits, all deployments will fail. For example, if pod limit and container limit is set to the following;
```yaml
resources:
  limits:
    cpu: 500m
    memory: 1Gi
```

OpenShift tries to reserve 2Gi for the 2 containers launched in the same pod. This obviously will exceed the pod limit set. If this is an issue, update the `resources.limits` values in the template before deploying. Or change it within your deployment configuration.
