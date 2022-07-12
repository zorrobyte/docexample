# Setting Up the Workspace Directory and Ansible Inventory File

In this section, you will create a directory on your local machine that will serve as your workspace. You will configure Ansible locally so that it can communicate with and execute commands on your remote servers. Once that’s done, you will create a `hosts` file containing inventory information such as the IP addresses of your servers and the groups that each server belongs to.

Out of your three servers, one will be the control plane with an IP displayed as `control_plane_ip`. The other two servers will be workers and will have the IPs `worker_1_ip` and `worker_2_ip`.

Create a directory named `~/kube-cluster` in the home directory of your local machine and `cd` into it:

```
mkdir ~/kube-cluster
cd ~/kube-cluster
```

This directory will be your workspace for the rest of the tutorial and will contain all of your Ansible playbooks. It will also be the directory inside which you will run all local commands.

Create a file named `~/kube-cluster/hosts` using `nano` or your favorite text editor:

```
nano ~/kube-cluster/hosts
```

Add the following text to the file, which will specify information about the logical structure of your cluster:

```
[control_plane]
control1 ansible_host=control_plane_ip ansible_user=root 

[workers]
worker1 ansible_host=worker_1_ip ansible_user=root
worker2 ansible_host=worker_2_ip ansible_user=root

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

You may recall that [_inventory files_](http://docs.ansible.com/ansible/latest/user\_guide/intro\_inventory.html) in Ansible are used to specify server information such as IP addresses, remote users, and groupings of servers to target as a single unit for executing commands. `~/kube-cluster/hosts` will be your inventory file and you’ve added two Ansible groups (**control plane** and **workers**) to it specifying the logical structure of your cluster.

In the **control plane** group, there is a server entry named “control1” that lists the control plane’s IP (`control_plane_ip`) and specifies that Ansible should run remote commands as the root user.

Similarly, in the **workers** group, there are two entries for the worker servers (`worker_1_ip` and `worker_2_ip`) that also specify the `ansible_user` as root.

The last line of the file tells Ansible to use the remote servers’ Python 3 interpreters for its management operations.

Save and close the file after you’ve added the text. If you are using `nano`, press `Ctrl+X`, then when prompted, `Y` and `Enter`.

Having set up the server inventory with groups, let’s move on to installing operating system level dependencies and creating configuration settings.
