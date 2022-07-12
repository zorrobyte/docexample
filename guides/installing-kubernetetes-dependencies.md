# Installing Kubernetetes Dependencies

In this section, you will install the operating-system-level packages required by Kubernetes with Ubuntu’s package manager. These packages are:

* Docker - a container runtime. It is the component that runs your containers. Kubernetes supports other runtimes, but Docker is still a popular and straightforward choice.
* `kubeadm` - a CLI tool that will install and configure the various components of a cluster in a standard way.
* `kubelet` - a system service/program that runs on all nodes and handles node-level operations.
* `kubectl` - a CLI tool used for issuing commands to the cluster through its API Server.

Create a file named `~/kube-cluster/kube-dependencies.yml` in the workspace:

```
nano ~/kube-cluster/kube-dependencies.yml
```

Add the following plays to the file to install these packages to your servers:

```
---
- hosts: all
  become: yes
  tasks:
   - name: create Docker config directory
     file: path=/etc/docker state=directory

   - name: changing Docker to systemd driver
     copy:
      dest: "/etc/docker/daemon.json"
      content: |
        {
        "exec-opts": ["native.cgroupdriver=systemd"]
        }

   - name: install Docker
     apt:
       name: docker.io
       state: present
       update_cache: true

   - name: install APT Transport HTTPS
     apt:
       name: apt-transport-https
       state: present

   - name: add Kubernetes apt-key
     apt_key:
       url: https://packages.cloud.google.com/apt/doc/apt-key.gpg
       state: present

   - name: add Kubernetes' APT repository
     apt_repository:
      repo: deb http://apt.kubernetes.io/ kubernetes-xenial main
      state: present
      filename: 'kubernetes'

   - name: install kubelet
     apt:
       name: kubelet=1.22.4-00
       state: present
       update_cache: true

   - name: install kubeadm
     apt:
       name: kubeadm=1.22.4-00
       state: present

- hosts: control_plane
  become: yes
  tasks:
   - name: install kubectl
     apt:
       name: kubectl=1.22.4-00
       state: present
       force: yes
```

The first play in the playbook does the following:

* Installs Docker, the container runtime, and configures a compatibility setting.
* Installs `apt-transport-https`, allowing you to add external HTTPS sources to your APT sources list.
* Adds the Kubernetes APT repository’s apt-key for key verification.
* Adds the Kubernetes APT repository to your remote servers’ APT sources list.
* Installs `kubelet` and `kubeadm`.

The second play consists of a single task that installs `kubectl` on your control plane node.

{% hint style="warning" %}
**Note:** While the Kubernetes documentation recommends you use the latest stable release of Kubernetes for your environment, this tutorial uses a specific version. This will ensure that you can follow the steps successfully, as Kubernetes changes rapidly and the latest version may not work with this tutorial. Although “xenial” is the name of Ubuntu 16.04, and this tutorial is for Ubuntu 20.04, Kubernetes is still referring to Ubuntu 16.04 package sources by default, and they are supported on 20.04 in this case.
{% endhint %}

Save and close the file when you are finished.

Next, run the playbook locally with the following command:

```
ansible-playbook -i hosts ~/kube-cluster/kube-dependencies.yml
```

On completion, you will receive output similar to the following:

```
OutputPLAY [all] ****

TASK [Gathering Facts] ****
ok: [worker1]
ok: [worker2]
ok: [control1]

TASK [create Docker config directory] ****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [changing Docker to systemd driver] ****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [install Docker] ****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [install APT Transport HTTPS] *****
ok: [control1]
ok: [worker1]
changed: [worker2]

TASK [add Kubernetes apt-key] *****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [add Kubernetes' APT repository] *****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [install kubelet] *****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [install kubeadm] *****
changed: [control1]
changed: [worker1]
changed: [worker2]

PLAY [control1] *****

TASK [Gathering Facts] *****
ok: [control1]

TASK [install kubectl] ******
changed: [control1]

PLAY RECAP ****
control1                     : ok=11    changed=9    unreachable=0    failed=0   
worker1                    : ok=9    changed=8    unreachable=0    failed=0  
worker2                    : ok=9    changed=8    unreachable=0    failed=0  
```

After running this playbook, Docker, `kubeadm`, and `kubelet` will be installed on all of the remote servers. `kubectl` is not a required component and is only needed for executing cluster commands. Installing it only on the control plane node makes sense in this context, since you will run `kubectl` commands only from the control plane. Note, however, that `kubectl` commands can be run from any of the worker nodes or from any machine where it can be installed and configured to point to a cluster.

All system dependencies are now installed. Let’s set up the control plane node and initialize the cluster.
