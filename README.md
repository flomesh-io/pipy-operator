# pipy-operator

CRD and Operator for running pipy in k8s.

## Quickstart

Run pipy on a fresh kubernetes:

~~~
kubectl apply -f etc/cert-manager-v1.1.0.yaml
kubectl apply -f artifact/pipy-operator.yaml
kubectl apply -f samples/standalone/001-echo.yaml
kubectl apply -f samples/ingress/008-routing.yaml
kubectl apply -f samples/sidecar/007-deployment-pipy.yaml
~~~

## Sample output

Output while running on k3s, for reference only:

~~~command
[root@crd pipy-operator]#  k3s -v
k3s version v1.20.0+k3s2 (2ea6b163)
go version go1.15.5

[root@crd pipy-operator]#  kubectl apply -f etc/cert-manager-v1.1.0.yaml

[root@crd ~]# kubectl get pods -A
NAMESPACE      NAME                                      READY   STATUS      RESTARTS   AGE
kube-system    helm-install-traefik-s77bg                0/1     Completed   5          9h
kube-system    metrics-server-86cbb8457f-d6hk2           1/1     Running     6          9h
kube-system    local-path-provisioner-7c458769fb-9ksqq   1/1     Running     7          9h
kube-system    svclb-traefik-8pb9c                       2/2     Running     4          9h
kube-system    coredns-854c77959c-gk9p8                  1/1     Running     2          9h
kube-system    traefik-6f9cbd9bd4-842d7                  1/1     Running     2          9h
cert-manager   cert-manager-6865f45f85-7gjcb             1/1     Running     0          9h
cert-manager   cert-manager-cainjector-fdbc9f44-8xv27    1/1     Running     0          9h
cert-manager   cert-manager-webhook-5d59497545-vdchs     1/1     Running     0          9h

[root@crd pipy-operator]#  kubectl apply -f artifact/pipy-operator.yaml

[root@crd pipy-operator]# kubectl apply -f artifact/pipy-operator.yaml
Warning: apiextensions.k8s.io/v1beta1 CustomResourceDefinition is deprecated in v1.16+, unavailable in v1.22+; use apiextensions.k8s.io/v1 CustomResourceDefinition
customresourcedefinition.apiextensions.k8s.io/proxies.flomesh.io created
namespace/flomesh-system created
customresourcedefinition.apiextensions.k8s.io/proxies.flomesh.io configured
role.rbac.authorization.k8s.io/flomesh-leader-election-role created
clusterrole.rbac.authorization.k8s.io/flomesh-manager-role created
clusterrole.rbac.authorization.k8s.io/flomesh-proxy-role created
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
clusterrole.rbac.authorization.k8s.io/flomesh-metrics-reader created
rolebinding.rbac.authorization.k8s.io/flomesh-leader-election-rolebinding created
clusterrolebinding.rbac.authorization.k8s.io/flomesh-manager-rolebinding created
clusterrolebinding.rbac.authorization.k8s.io/flomesh-proxy-rolebinding created
service/flomesh-controller-manager-metrics-service created
service/flomesh-webhook-service created
deployment.apps/flomesh-controller-manager created
certificate.cert-manager.io/flomesh-serving-cert created
issuer.cert-manager.io/flomesh-selfsigned-issuer created
Warning: admissionregistration.k8s.io/v1beta1 MutatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 MutatingWebhookConfiguration
mutatingwebhookconfiguration.admissionregistration.k8s.io/flomesh-mutating-webhook-configuration created
Warning: admissionregistration.k8s.io/v1beta1 ValidatingWebhookConfiguration is deprecated in v1.16+, unavailable in v1.22+; use admissionregistration.k8s.io/v1 ValidatingWebhookConfiguration
validatingwebhookconfiguration.admissionregistration.k8s.io/flomesh-validating-webhook-configuration created

[root@crd pipy-operator]# kubectl get pods -A
NAMESPACE        NAME                                          READY   STATUS      RESTARTS   AGE
kube-system      helm-install-traefik-s77bg                    0/1     Completed   5          9h
kube-system      metrics-server-86cbb8457f-d6hk2               1/1     Running     6          9h
kube-system      local-path-provisioner-7c458769fb-9ksqq       1/1     Running     7          9h
kube-system      svclb-traefik-8pb9c                           2/2     Running     4          9h
kube-system      coredns-854c77959c-gk9p8                      1/1     Running     2          9h
kube-system      traefik-6f9cbd9bd4-842d7                      1/1     Running     2          9h
cert-manager     cert-manager-6865f45f85-7gjcb                 1/1     Running     0          9h
cert-manager     cert-manager-cainjector-fdbc9f44-8xv27        1/1     Running     0          9h
cert-manager     cert-manager-webhook-5d59497545-vdchs         1/1     Running     0          9h
flomesh-system   flomesh-controller-manager-55fb9565bb-c46fc   2/2     Running     0          79s

[root@crd pipy-operator]#  kubectl apply -f config/samples/standalone/001-echo.yaml

[root@pipy pipy-operator]# kubectl get pods -A
NAMESPACE        NAME                                          READY   STATUS      RESTARTS   AGE
kube-system      metrics-server-86cbb8457f-xhtqv               1/1     Running     14         122m
kube-system      local-path-provisioner-7c458769fb-hqlth       1/1     Running     14         122m
kube-system      helm-install-traefik-qfs64                    0/1     Completed   14         122m
kube-system      svclb-traefik-f7r94                           2/2     Running     0          98m
kube-system      traefik-6f9cbd9bd4-mfs5r                      1/1     Running     0          98m
kube-system      coredns-854c77959c-5gf4b                      1/1     Running     1          122m
cert-manager     cert-manager-cainjector-fdbc9f44-6wdr9        1/1     Running     0          96m
cert-manager     cert-manager-6865f45f85-5dkdp                 1/1     Running     0          96m
cert-manager     cert-manager-webhook-5d59497545-75njv         1/1     Running     0          96m
flomesh-system   flomesh-controller-manager-55fb9565bb-f888q   2/2     Running     0          94m
default          pipy-001-dp-574f8dfc49-mc6j4                  1/1     Running     0          3m26s

[root@pipy pipy-operator]# kubectl get svc -A
NAMESPACE        NAME                                         TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)                      AGE
default          kubernetes                                   ClusterIP      10.43.0.1       <none>           443/TCP                      123m
kube-system      kube-dns                                     ClusterIP      10.43.0.10      <none>           53/UDP,53/TCP,9153/TCP       123m
kube-system      metrics-server                               ClusterIP      10.43.2.237     <none>           443/TCP                      123m
kube-system      traefik-prometheus                           ClusterIP      10.43.21.21     <none>           9100/TCP                     98m
kube-system      traefik                                      LoadBalancer   10.43.44.27     192.168.122.52   80:31331/TCP,443:31270/TCP   98m
cert-manager     cert-manager                                 ClusterIP      10.43.215.130   <none>           9402/TCP                     97m
cert-manager     cert-manager-webhook                         ClusterIP      10.43.161.71    <none>           443/TCP                      97m
flomesh-system   flomesh-controller-manager-metrics-service   ClusterIP      10.43.37.135    <none>           8443/TCP                     94m
flomesh-system   flomesh-webhook-service                      ClusterIP      10.43.129.127   <none>           443/TCP                      94m
default          pipy-001-svc                                 ClusterIP      10.43.132.96    <none>           8080/TCP                     4m7s

[root@pipy pipy-operator]# curl -X POST http://10.43.132.96:8080/ --data "hello, pipy proxy"
hello, pipy proxy
~~~
