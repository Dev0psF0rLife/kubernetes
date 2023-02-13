# Kubernetes
This repository will help you cross barriers with kubernetes

## ISTIO ##

**INSTALLATION:** 
```
1. wget https://sarma.prz.hz.eu-west-1.aws.thales:443/artifactory/CASPR-tools/istio-1.16.0-linux-amd64.tar.gz
2. tar -zxvf istio-1.16.0-linux-amd64.tar.gz
3. cd istio-1.16.0-linux-amd64.tar.gz
4. export PATH=$PWD/bin:$PATH
```
Great, now you are able to execute istioctl commands.

**Apply demo profile** _(this step will deploy a template with istio ingress,egress and istiod gateways. Also, this step will configure services for istio gateways)._
```
1. istioctl install --set profile=demo -y --set hub=docker-remote.sarma.prz.hz.eu-west-1.aws.thales/istio
```

## CERT-MANAGER ##
**INSTALLATION:**
(this step will deploy cert-manager, cainjector and webhook pods, also, will configure cert-manager services)
```
1. kubectl apply -f cert-manager/cert-manager-1.10.yml 
```
## ISSUER ##
_(check if you are applying issuers on istio namespace). This step will create the request into vault, contains approle method authentication._
```
1. kubectl apply -f issuers/issuer.yml 
```
## SECRET ## 
```
1. kubectl apply -f secrets/secret.yml 
```
_(this secret, contains the secret id encoded on base64, this secret id must to be created before on vault with policy permissions.)_

## CERTIFICATES ##
_(On this step, you'll create a certificate with a dns name)._

**CREATION:**
```
1. kubectl apply -f certificates/certificate.yml 
```

## LOKI ##
_(On this step, you'll create a deployment of loki, it will storage your logs on filesystem, could be configured to use persistent storages or s3 buckets, on this example we're storaging on filesystem.)._
```
1. kubectl apply -f loki/loki.yaml 
```
## PROMTAIL ##
_On this step we will deploy the promtail-server, promtail is a agent who will catch logs on all namespaces, this one is set as a daemonset(replicas created on all nodes)._
```
1. kubectl apply -f promtail/promtail.yaml 
```