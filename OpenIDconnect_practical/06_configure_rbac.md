# Configuring Role-absed access control in server


- some yaml beautifier
	https://jsonformatter.org/yaml-formatter
	https://onlineyamltools.com/prettify-yaml


#### 1. add role and clusterolebidning to a group

- these are the groups available via claim
```
  "groups": [
    "kubernetes_role",
    "manage-account",
    "manage-account-links",
    "view-profile"
  ],
  
```
source: https://developer.okta.com/blog/2021/11/08/k8s-api-server-oidc

	-> there is a built in admin role cluster-admin which could be used for pull access

- we create a restricted pods-readonly role and attach it to kubernetes_role

```
kubectl apply -f - <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  namespace: default
  name: pods-readonly
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
EOF
```

- Next:

```			 
kubectl apply -f - <<EOF
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: pods-readonly-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: pods-readonly
subjects:
- kind: Group
  name: kubernetes_role
EOF
``
			
:arrow_right: k get ClusterRoleBinding | grep pods

:arrow_right: k get ClusterRole | grep pods


