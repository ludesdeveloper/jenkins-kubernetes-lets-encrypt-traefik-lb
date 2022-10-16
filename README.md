# Jenkins Kubernetes Let's Encrypt

<p align="center">
<img src="pic/ludes.png" width="500">
</p>

This is sample kubernetes installation for jenkins.

### **What You Will Get**

1. Jenkins installed on kubernetes
2. Ingress(traefik) to jenkins namespace
3. Secure connection (https) to your jenkins namespace using Let's Encrypt

### **Jenkins Installation**

1. Clone repo

```
git clone https://github.com/ludesdeveloper/jenkins-kubernetes-lets-encrypt-traefik-lb.git
```

2. Change directory

```
cd jenkins-kubernetes-lets-encrypt-traefik-lb
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

### **Ingress**

1. Create Traefik namespace

```
kubectl create namespace traefik
```

2. Add Traefik repo

```
helm repo add traefik https://helm.traefik.io/traefik
```

3. Update repo

```
helm repo update
```

4. Install Traefik

```
helm install traefik traefik/traefik -n traefik
```

5. Install ingress, please change with your ingress controller ip address
   > I am using nip.io for dns

   > Load balancer will give you public ip, you can use it to set nipIP

```
helm install ingress ./ingress --set nipIPAddress=111-111-111-111
```

### **Jenkins Credential**

1. Get jenkins password (Username default is 'admin')

```
jsonpath="{.data.jenkins-admin-password}"
secret=$(kubectl get secret -n jenkins jenkins -o jsonpath=$jsonpath)
echo $(echo $secret | base64 --decode)
```

### **Sources**

1. [K3S Let's Encrypt](https://k3s.rocks/https-cert-manager-letsencrypt/)
2. [Installing Jenkins on Kubernetes](https://www.jenkins.io/doc/book/installing/kubernetes/)
