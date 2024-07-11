
Step 1 Install Docker Desktop 


Step 2


Step 3 Deploy The Dashboard UI

`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml`

if failed use official site using Helm 



Step 4: Create Admin User and Get Bearer Token

save as dashboard-adminuser.yaml

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```

`kubectl apply -f dashboard-adminuser.yaml`


Get bearer Token

kubectl -n kubernetes-dashboard create token admin-user


Step 5: Start Proxy

kubectl proxy

Step 6: Access kubernetes Dashboard 




Step 7: Install Istio and Kial

[Follow here](https://istio.io/latest/docs/setup/getting-started/)



#Resources 

[How to Run Kubernetes Locally in Docker Desktop](https://sudipta-deb.in/2023/02/how-to-run-kubernetes-locally-in-docker-desktop.html)

[Udemy Course ](https://www.udemy.com/course/istio-hands-on-for-kubernetes/?couponCode=ST9MT71624)
