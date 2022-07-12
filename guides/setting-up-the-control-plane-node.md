# Setting Up the Control Plane Node

In this section, you will set up the control plane node. Before creating any playbooks, however, it’s worth covering a few concepts such as _Pods_ and _Pod Network Plugins_, since your cluster will include both.

A pod is an atomic unit that runs one or more containers. These containers share resources such as file volumes and network interfaces in common. Pods are the basic unit of scheduling in Kubernetes: all containers in a pod are guaranteed to run on the same node that the pod is scheduled on.

Each pod has its own IP address, and a pod on one node should be able to access a pod on another node using the pod’s IP. Containers on a single node can communicate easily through a local interface. Communication between pods is more complicated, however, and requires a separate networking component that can transparently route traffic from a pod on one node to a pod on another.

This functionality is provided by pod network plugins. For this cluster, you will use [Flannel](https://github.com/coreos/flannel), a stable and performant option.

Create an Ansible playbook named `control-plane.yml` on your local machine:

```
nano ~/kube-cluster/control-plane.yml
```

Add the following play to the file to initialize the cluster and install Flannel:

```
--- 
- 
  become: true
  hosts: control_plane
  tasks: 
    - 
      args: 
        chdir: $HOME
        creates: cluster_initialized.txt
      name: "initialize the cluster"
      shell: "kubeadm init --pod-network-cidr=10.244.0.0/16 >> cluster_initialized.txt"
    - 
      become: true
      become_user: ubuntu
      file: 
        mode: 493
        path: $HOME/.kube
        state: directory
      name: "create .kube directory"
    - 
      copy: 
        dest: /home/ubuntu/.kube/config
        owner: ubuntu
        remote_src: true
        src: /etc/kubernetes/admin.conf
      name: "copy admin.conf to user's kube config"
    - 
      args: 
        chdir: $HOME
        creates: pod_network_setup.txt
      become: true
      become_user: ubuntu
      name: "install Pod network"
      shell: "kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml >> pod_network_setup.txt"
```

Here’s a breakdown of this play:

* The first task initializes the cluster by running `kubeadm init`. Passing the argument `--pod-network-cidr=10.244.0.0/16` specifies the private subnet that the pod IPs will be assigned from. Flannel uses the above subnet by default; we’re telling `kubeadm` to use the same subnet.
* The second task creates a `.kube` directory at `/home/ubuntu`. This directory will hold configuration information such as the admin key files, which are required to connect to the cluster, and the cluster’s API address.
* The third task copies the `/etc/kubernetes/admin.conf` file that was generated from `kubeadm init`to your non-root user’s home directory. This will allow you to use `kubectl` to access the newly-created cluster.
* The last task runs `kubectl apply` to install `Flannel`. `kubectl apply -f descriptor.[yml|json]` is the syntax for telling `kubectl` to create the objects described in the `descriptor.[yml|json]` file. The `kube-flannel.yml` file contains the descriptions of objects required for setting up `Flannel` in the cluster.

Save and close the file when you are finished.

Run the playbook locally with the following command:

```
ansible-playbook -i hosts ~/kube-cluster/control-plane.yml
```

On completion, you will see output similar to the following:

```
Output
PLAY [control1] ****

TASK [Gathering Facts] ****
ok: [control1]

TASK [initialize the cluster] ****
changed: [control1]

TASK [create .kube directory] ****
changed: [control1]

TASK [copy admin.conf to user's kube config] *****
changed: [control1]

TASK [install Pod network] *****
changed: [control1]

PLAY RECAP ****
control1                     : ok=5    changed=4    unreachable=0    failed=0  
```

To check the status of the control plane node, SSH into it with the following command:

```
ssh ubuntu@control_plane_ip
```

Once inside the control plane node, execute:

```
kubectl get nodes
```

You will now see the following output:

```
OutputNAME     STATUS   ROLES                  AGE   VERSION
control1   Ready    control-plane,master   51s   v1.22.4
```

{% hint style="info" %}
**Note:** As of Ubuntu 20.04, kubernetes is in the process of updating their old terminology. The node we’ve referred to as `control-plane` throughout this tutorial used to be called the `master` node, and occasionally you’ll see kubernetes assigning both roles simultaneously for compatibility reasons.
{% endhint %}

The output states that the `control-plane` node has completed all initialization tasks and is in a `Ready`state from which it can start accepting worker nodes and executing tasks sent to the API Server. You can now add the workers from your local machine.
