# How to upgrade ingress-nginx controller without downtime


## Step 1: Install existing ingress-nginx controller with older version
  ```bash
  # disable controller service
  helm upgrade --install ingress-nginx-new ingress-nginx \
--repo https://kubernetes.github.io/ingress-nginx \
--namespace ingress-nginx \
--set controller.service.enabled=false
  ```

## Step 2: create new ingress-nginx controller
   - create new nginx controller in the same namespace with new app label
   ```bash
   # disable controller service and admissionWebhooks
   helm upgrade --install ingress-nginx-new ingress-nginx \
--repo https://kubernetes.github.io/ingress-nginx \
--namespace ingress-nginx \
--set controller.service.enabled=false \
--set controller.admissionWebhooks.enabled=false
```
> Note: admissionWebhooks is not supported in GKE
   
## Step 3: change old nginx service point to new nginx controller label
   - change old nginx service point to new nginx controller label

## Step 4: delete old nginx controller
```bash
helm uninstall ingress-nginx -n ingress-nginx
```




### How to set custom configuration on helm chart
```bash
# fetch helm chart to see setting
helm fetch ingress-nginx/ingress-nginx --repo https://kubernetes.github.io/ingress-nginx

# example of set custom configuration
helm upgrade --install ingress-nginx-new-nlb ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.ingressClassResource.name="ingress-nginx-new-nlb" \
  --set controller.ingressClass="ingress-nginx-new-nlb" \
  --set controller.service.annotations."service\.beta\.kubernetes\.io/aws-load-balancer-type"="nlb"
```


> Note: Setting default IngressClass
```yaml
specific IngressClass -> default backend
  annotations:
    ingressclass.kubernetes.io/is-default-class: 'true'
```