



## Installation

1. `sudo kn quickstart kind `

2.  Install the Knative CLI

3. Install the Knative quickstart plugin

4. `netstat -tnlp | grep 80`





## Installing the Knative Serving Component

The first step is to install the custom resource definitions (CRDs) 


```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.4.0/serving-crds.yaml
```

 install the core components of Knative Serving.

```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.4.0/serving-core.yaml
```

### Installing a Networking Layer

Knative requires a networking layer to manage the traffic and request
 connections to your containerized applications. The Knative Serving 
component supports different network layers such as [Kourier](https://github.com/knative-sandbox/net-kourier), Istio, or Contour. The following command installs Kourier v1.4.0:

```bash
kubectl apply -f https://github.com/knative/net-kourier/releases/download/knative-v1.4.0/kourier.yaml
```

After installing Kourier in your Kubernetes cluster, the following command 
configures Knative Serving, and sets Kourier as the default networking 
layer:

```bash
kubectl patch configmap/config-network \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"ingress.class":"kourier.ingress.networking.knative.dev"}}'
```



### Verifying the Installation of Knative Serving



```bash
kubectl get pods -n knative-serving
```

### Configuring the DNS (optional)



```bash
kubectl apply -f https://github.com/knative/serving/releases/download/knative-v1.4.0/serving-default-domain.yaml
```

The preceding command creates a Job that configures Knative Serving to use `sslip.io` as the DNS



## Installing the Knative Eventing Component

The first step is to install the custom resource definitions (CRDs) 


```bash
kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.4.0/eventing-crds.yaml
```

The next step is to install the core components of Knative Eventing.

```bash
kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.4.0/eventing-core.yaml
```

The preceding command creates a namespace called `knative-eventing` and deploys the core



### Verifying the Installation of Knative Eventing



```bash
kubectl get pods -n knative-eventing
```





### Installing a Default Messaging Layer for Events



```bash
kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.4.0/in-memory-channel.yaml
```



### Installing a Broker Layer

```
kubectl apply -f https://github.com/knative/eventing/releases/download/knative-v1.4.0/mt-channel-broker.yaml
```



## Creating A service

from `https://github.com/skshahriarahmedraka/Simple-HTTP-Server-in-Knative`  repository , 

build docker image : 

`sudo docker build -t  shahriarraka/simple-http-knative .`

`sudo docker push  shahriarraka/simple-http-knative:latest`



## deploy a service

```
sudo kn service create simple-http-knative --image=shahriarraka/simple-http-knative:latest --port=8090

```

output :

```
Creating service 'simple-http-knative' in namespace 'default':

  0.060s The Route is still working to reflect the latest desired specification.
  0.072s ...
  0.103s Configuration "simple-http-knative" is waiting for a Revision to become ready.
172.847s ...
172.945s Ingress has not yet been reconciled.
173.028s Waiting for load balancer to be ready
173.192s Ready to serve.

Service 'simple-http-knative' created to latest revision 'simple-http-knative-00001' is available at URL:
http://simple-http-knative.default.127.0.0.1.sslip.io

```



![](./screenshot/Screenshot%20from%202023-07-25%2014-55-58.png)



From browser :

![](./screenshot/Screenshot%20from%202023-07-25%2014-55-39.png)
