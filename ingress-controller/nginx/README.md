# nginx-ingress-controller

## Set up
Reference: https://kubernetes.github.io/ingress-nginx/deploy/#bare-metal-clusters

Followed Baremetal instrucitons for laptop

$ kubectl get svc ingress-nginx-controller -n ingress-nginx
NAME                       TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller   NodePort   10.152.183.156   <none>        80:30969/TCP,443:31519/TCP   4h

==> ingress controller available at 30969 / 31519

## Set up sample services

kubectl create deployment hello --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080

kubectl expose deployment hello --type=ClusterIP --port=8080

kubectl create deployment nginx --image=nginx

kubectl expose deployment nginx --type=ClusterIP --port=80


## Create Ingress rules

kubectl apply -f ingress.yaml

N.B. ingress resource should be placed inside the same namespace of the backend resource

## Test

curl -H "Host:nginx.org" localhost:30969
curl -H "Host:hello.org" localhost:30969

Or, we can use a browser extension to add/manage Header 

curl localhost:30969/healthz --> Default backend service (just gives Response code 200)


## Annotation, Basic Auth
 
htpasswd -c auth admin    (or use the file ./auth, it has admin/pass)

kubectl create secret generic basic-auth --from-file=username.txt --from-file=password.txt

Now the nginx is secured with Basic Auth : 
curl -v http://localhost:30969 -H 'Host: nginx.org' -> gives 401 Authorization Required
 curl -v http://localhost:30969 -H 'Host: nginx.org' -u  'admin:pass' -> works 

## 


openssl genrsa -out ca.key 2048
openssl req -x509 -new -nodes  -days 365 -key ca.key -out ca.crt -subj "/CN=hello.org"
kubectl create secret tls k8s-secret --key ca.key --cert ca.crt -n kube-system
