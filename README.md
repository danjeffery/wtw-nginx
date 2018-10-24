# Vagrant up kubernetes cluster using Ubuntu 18.04 and kubeadm
This will spin up a 3-node, 1-master kubernetes cluster on Ubuntu 18.04 and deploy nginx in a load-balanced config.
It uses https://github.com/cloudnativelabs/kube-router for the pod network.

## Requirements on Fedora:
sudo dnf install vagrant vagrant-sshfs libvirt
sudo gpasswd -a $USER libvirt

## Usage:
```
vagrant up
```
A 'vagrant up' will spin up a new 3 node + 1 master cluster.

### Once your cluster is running you can login to k8s-master and run kubectl as root:
```
vagrant ssh k8s-master1
sudo -i
kubectl get nodes -o wide
kubectl get pods -l app=nginx
kubectl describe service nginx-svc
```

### To test that the default.conf is running as expected run the following commands on k8s-master1 ###
nodePort = $(kubectl describe service nginx-svc | awk '/^NodePort:/ { print substr($3,0,5) }')
curl -k -H'Host: www.example.com' http://10.4.2.11:$nodePort
curl -k http://10.4.2.11:$nodePort
