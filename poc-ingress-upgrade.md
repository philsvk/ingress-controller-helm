# POC Ingress Upgrade

### Install IngressController

1. Install IngressController without IngressClass
```bash
helm upgrade --install ingress-nginx ingress-nginx \
--repo https://kubernetes.github.io/ingress-nginx \
--namespace ingress-nginx --create-namespace \
--set controller.ingressClassResource.enabled=false \
--set controller.admissionWebhooks.enabled=false
```

### Install NewIngressController

2. Install New IngressController without ServiceController and admissionWebhooks
```bash
helm upgrade --install ingress-nginx-new ingress-nginx \
--repo https://kubernetes.github.io/ingress-nginx \
--namespace ingress-nginx \
--set controller.service.enabled=false \
--set controller.admissionWebhooks.enabled=false
```


3. Change Old ServiceController Selector point to New IngressController
    `app.kubernetes.io/instance: ingress-nginx-new`



kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission

kubectl get -A ValidatingWebhookConfiguration


https://stackoverflow.com/questions/61365202/nginx-ingress-service-ingress-nginx-controller-admission-not-found