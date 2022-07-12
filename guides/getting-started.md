# Getting Started

### Goals <a href="#goals" id="goals"></a>

Your cluster will include the following physical resources:

* **One** [**Control Plane**](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) **node**

The control plane node (a _node_ in Kubernetes refers to a server) is responsible for managing the state of the cluster. It runs [Etcd](https://github.com/coreos/etcd), which stores cluster data among components that schedule workloads to worker nodes.

* **Two worker nodes**

Worker nodes are the servers where your _workloads_ (i.e. containerized applications and services) will run. A worker will continue to run your workload once they’re assigned to it, even if the control plane goes down once scheduling is complete. A cluster’s capacity can be increased by adding workers.

After completing this guide, you will have a cluster ready to run containerized applications, provided that the servers in the cluster have sufficient CPU and RAM resources for your applications to consume. Almost any traditional Unix application including web applications, databases, daemons, and command line tools can be containerized and made to run on the cluster. The cluster itself will consume around 300-500MB of memory and 10% of CPU on each node.

Once the cluster is set up, we will deploy the web server [Nginx](https://nginx.org/en/) to it to ensure that it is running workloads correctly. Finally, we will cost optimize and monitor our cluster with [KubeCost](https://www.kubecost.com)!

## The basics

* An SSH key pair on your local Linux/macOS/BSD machine. If you haven’t used SSH keys before, you can learn how to set them up by following [this explanation of how to set up SSH keys on your local machine](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys#generating-and-working-with-ssh-keys).
* Three servers running Ubuntu 20.04 with at least 2GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user with your SSH key pair.

{% hint style="info" %}
**Note:** If you haven’t SSH’d into each of these servers at least once prior to following this tutorial, you may be prompted to accept their host fingerprints at an inconvenient time later on. You should do this now.
{% endhint %}

* Ansible installed on your local machine. If you’re running Ubuntu 20.04 as your OS, follow the “Step 1 - Installing Ansible” section in [How to Install and Configure Ansible on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-20-04#step-1-%E2%80%94-installing-ansible) to install Ansible. For installation instructions on other platforms like macOS or Rocky Linux, follow the [official Ansible installation documentation](http://docs.ansible.com/ansible/latest/installation\_guide/intro\_installation.html#installing-the-control-machine).
* Familiarity with Ansible playbooks. For review, check out [Configuration Management 101: Writing Ansible Playbooks](https://www.digitalocean.com/community/tutorials/configuration-management-101-writing-ansible-playbooks).
* Knowledge of how to launch a container from a Docker image. Look at “Step 5 — Running a Docker Container” in [How To Install and Use Docker on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04#step-5-%E2%80%94-running-a-docker-container) if you need a refresher.
* A [DigitalOcean](https://www.digitalocean.com) account
