# Create the Base Box 

First of all we need to create the base box having k8s installed. 

In the `base_box_for_vagrant_mini_k8s`  directory there is a Vagrantfile which installs the kubernetes v1.14.3 using kubeadm. 



```
   cd base_box_for_vagrant_mini_k8s
   vagrant up
```
Save the created box with name k8s-vagrant-1.14.3.box

```
vagrant package --output k8s-vagrant-1.14.3.box 
#vagrant box add k8s-vagrant k8s-vagrant-1.14.3.box --box-version 1.14.3
#version constraint will not work if we add from direct box file
vagrant box add k8s-vagrant k8s-vagrant-1.14.3.box --box-version 1.14.3
```



once the  box is packaged properly destory the vagrant 

```
cd base-box/
[kc:none][aws:none][jmemon@C02XH66YJG5M base-box]$ vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Destroying VM and associated drives...
```



# Start the k8s vagrant node 

Now you can start the main k8s vagrant machie, there is a Vagrantfile which is based on the box we created in previous step. 



```
   cd vagrant_mini_k8s
   vagrant up
   
$ vagrant ssh
INFO[0000] Host 127.0.0.1
INFO[0000]   Port 2222
INFO[0000]   # HostName: 127.0.0.1
Last login: Wed Jun 12 15:51:30 2019 from 10.0.2.2
[vagrant@mini-k8s ~]$ kubectl get nodes
NAME       STATUS   ROLES    AGE   VERSION
mini-k8s   Ready    master   37m   v1.14.3
[vagrant@mini-k8s ~]$ kubectl get pods -n kube-system
NAME                               READY   STATUS    RESTARTS   AGE
coredns-fb8b8dccf-l5gpr            1/1     Running   1          36m
coredns-fb8b8dccf-q8ph5            1/1     Running   1          36m
etcd-mini-k8s                      1/1     Running   2          36m
kube-apiserver-mini-k8s            1/1     Running   2          36m
kube-controller-manager-mini-k8s   1/1     Running   2          36m
kube-flannel-ds-amd64-6txnm        1/1     Running   1          36m
kube-proxy-m5kgs                   1/1     Running   1          36m
kube-scheduler-mini-k8s            1/1     Running   2          36m
```



# Running the kubectl from your local machine 

You can copy the config file from the vagrant box and copy to your local machine and run the `kubectl` from your local machine. 

```
 vagrant ssh  -c 'cat ~/.kube/config' > kube-config
INFO[0000] Host 127.0.0.1
INFO[0000]   Port 2222
INFO[0000]   # HostName: 127.0.0.1
Connection to 127.0.0.1 closed.
[kc:none][aws:none][jmemon@C02XH66YJG5M k8s-vagrant]$ kubectl get nodes --kubeconfig kube-config
NAME       STATUS   ROLES    AGE   VERSION
mini-k8s   Ready    master   45m   v1.14.3
```



For setting the —kubeconfig for the session export the KUBECONFIG variable like this 

```
$ vagrant ssh  -c 'cat ~/.kube/config' > kube-config
INFO[0000] Host 127.0.0.1
INFO[0000]   Port 2222
INFO[0000]   # HostName: 127.0.0.1
Connection to 127.0.0.1 closed.
[kc:none][aws:none][jmemon@C02XH66YJG5M k8s-vagrant]$ export KUBECONFIG=kube-config
[kc:none][aws:none][jmemon@C02XH66YJG5M k8s-vagrant]$ kubectl get nodes
NAME       STATUS   ROLES    AGE   VERSION
mini-k8s   Ready    master   47m   v1.14.3
```

