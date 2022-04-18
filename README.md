# nginx-openshift-workshop

The repo contains folders which are all self sufficient. You may apply all the yaml in that folder in one go to demonstrate specific use case.

These manifests are based on NGINX Ingress Controller 1.10

Please refer to NGINX official repo for comprehensive list of examples:

https://github.com/nginxinc/kubernetes-ingress 


# 01-nginx-ingress-operator

To deploy:

`oc apply -f 01-nginx-ingress-operator -n namespace`

# 02-virtualserver-resource

To deploy:

`oc apply -f 02-nginx-ingress-operator -n namespace`

To test:

```
curl cafe.example.com/tea
curl cafe.example.com/coffee
```

# 03-custom-error-handler

To deploy:

`oc apply -f 03-custom-error-handler -n namespace`

To test:

`oc scale --replicas=0 deployment coffee -n namespace`

`curl http://cafe.example.com/coffee`

Expected response:

`<center><h1>oops this is embarrassing! We will be back shortly!</h1></center>%`

# 04-bluegreen

To deploy:

`oc apply -f 04-bluegreen -n namespace`

# 05-trafficsplit

To deploy:

`oc apply -f 05-trafficsplit -n namespace`

# 06-ratelimit

To deploy:

`oc apply -f 06-ratelimit -n namespace`

To test:

```
#for i in {1..10}; do curl -k -I -H "user-id:$1" --resolve cafe.example.com:443:10.1.1.5 https://cafe.example.com/coffee; done
for i in {1..10}; do curl -k -I -H "user-id:$100" --resolve cafe.example.com:443:10.1.1.5 https://cafe.example.com/coffee; done
#for i in {1..10}; do curl -k -I -H "user-id:$100" --resolve cafe.example.com:443:10.1.1.5 https://cafe.example.com/tea; done
```
Expected response:

`HTTP/1.1 503 Service Temporarily Unavailable`

