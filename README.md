# Jenkins Kubernetes Let's Encrypt

<p align="center">
<img src="pic/ludes.png" width="500">
</p>

This is sample kubernetes installation for jenkins.

### **What You Will Get**

1. Jenkins installed on kubernetes
2. Ingress(traefik) to jenkins namespace
3. Secure connection (https) to your jenkins namespace using Let's Encrypt

### **How To**

1. Clone repo

```
git clone https://github.com/ludesdeveloper/jenkins-kubernetes-lets-encrypt.git
```

2. Change directory

```
cd jenkins-kubernetes-lets-encrypt
```

3. Create namespace

```
kubectl create namespace jenkins
```

4. Install operator dependencies

```
kubectl apply -f operator/
```

5. Install jenkins operator

```
helm repo add jenkinsci https://charts.jenkins.io
helm repo update
```

6. Jenkins objects

```
helm install jenkins-objects ./jenkins-objects -n jenkins
```

7. Create jenkins instance

```
chart=jenkinsci/jenkins
helm install jenkins -n jenkins -f jenkins-values.yaml $chart
```

8. Install ingress, please change with your ingress controller ip address
   > I am using nip.io for dns

```
helm install ingress ./ingress --set nipIPAddress=111-111-111-111
```

9. Get jenkins password

```
jsonpath="{.data.jenkins-admin-password}"
secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)
echo $(echo $secret | base64 --decode)
```

### **Sources**

1. [K3S Let's Encrypt](https://k3s.rocks/https-cert-manager-letsencrypt/)
2. [Installing Jenkins on Kubernetes](https://www.jenkins.io/doc/book/installing/kubernetes/)
