# Overview

This repository contains the modified code for running WSO2 APIM and APIM Analytics locally using Minikube

## Requirements

1. Minikube
2. Kubectl

Start minikube and get the minikube IP address. You might need to specify the driver to get the minikube with IP address. Use the below command to initiate a minikube cluster

```
minikube start --vm-driver=hyperkit
```

To get the minikube ip, ue the below command

```
minikube ip
```

Grab the minikube ip and add the entry in hosts file with **am.com** as the **hostname**

```
Mac/Linux: etc/hosts 
Windows: C:\Windows\System32\Drivers\etc\hosts
```

Example hosts file in /etc/hosts

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
192.168.64.29   am.com
127.0.0.1       localhost
```
 
### Steps to install APIM with Analytics

```
cd simple
```

#### 1. create namespace and service account

```
cd kubernetes-basics
kubectl apply -f namespace.yaml        
kubectl apply -f svcaccount.yaml
```

#### 2. create mysql instance

```
cd ../kubernetes-apim-mysql
kubectl apply -f wso2apim-mysql-conf.yaml
kubectl apply -f wso2apim-mysql-service.yaml
kubectl apply -f wso2apim-mysql-deployment.yaml
```

#### 3. create analytics component

```
cd ../kubernetes-apim-analytics
```

##### Worker component (first create the worker component)

```
cd worker
kubectl apply -f wso2apim-analytics-worker-conf.yaml
kubectl apply -f wso2apim-analytics-worker-service.yaml
kubectl apply -f wso2apim-analytics-worker-deployment.yaml
```

##### Dashboard component

```
cd ../dashboard
kubectl apply -f wso2am-pattern-1-analytics-dashboard-conf.yaml
kubectl apply -f wso2am-pattern-1-analytics-dashboard-service.yaml
kubectl apply -f wso2am-pattern-1-analytics-dashboard-deployment.yaml
```

#### 4. create API Manager component

##### Edit the conf file with minikube ip and host name

```
cd ../../kubernetes-apim
```

Search for **192.168.64.29** in the wso2apim-conf.yaml file and replace all the instances with your minikube IP address.

```
kubectl apply -f wso2apim-conf.yaml
kubectl apply -f wso2apim-service.yaml
kubectl apply -f wso2apim-deployment.yaml
```

#### 5. Login URLs of Publisher, Devportal and Analytics-dashboard

https://am.com:30443/publisher

https://am.com:30443/devportal

https://am.com:30646/analytics-dashboard

Remember: Gateway nodeport is 30444
