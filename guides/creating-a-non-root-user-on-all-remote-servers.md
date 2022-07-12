# Creating a Non-Root User on All Remote Servers

In this section you will create a non-root user with sudo privileges on all servers so that you can SSH into them manually as an unprivileged user. This can be useful if, for example, you would like to see system information with commands such as `top/htop`, view a list of running containers, or change configuration files owned by root. These operations are routinely performed during the maintenance of a cluster, and using a non-root user for such tasks minimizes the risk of modifying or deleting important files or unintentionally performing other dangerous operations.

Create a file named `~/kube-cluster/initial.yml` in the workspace:

```
nano ~/kube-cluster/initial.yml
```

Next, add the following _play_ to the file to create a non-root user with sudo privileges on all of the servers. A play in Ansible is a collection of steps to be performed that target specific servers and groups. The following play will create a non-root sudo user:

```
---
- hosts: all
  become: yes
  tasks:
    - name: create the 'ubuntu' user
      user: name=ubuntu append=yes state=present createhome=yes shell=/bin/bash

    - name: allow 'ubuntu' to have passwordless sudo
      lineinfile:
        dest: /etc/sudoers
        line: 'ubuntu ALL=(ALL) NOPASSWD: ALL'
        validate: 'visudo -cf %s'

    - name: set up authorized keys for the ubuntu user
      authorized_key: user=ubuntu key="{{item}}"
      with_file:
        - ~/.ssh/id_rsa.pub
```

Here’s a breakdown of what this playbook does:

* Creates the non-root user `ubuntu`.
* Configures the `sudoers` file to allow the `ubuntu` user to run `sudo` commands without a password prompt.
* Adds the public key in your local machine (usually `~/.ssh/id_rsa.pub`) to the remote `ubuntu` user’s authorized key list. This will allow you to SSH into each server as the `ubuntu` user.

Save and close the file after you’ve added the text.

Next, run the playbook locally:

```
ansible-playbook -i hosts ~/kube-cluster/initial.yml
```

The command will complete within two to five minutes. On completion, you will see output similar to the following:

```
OutputPLAY [all] ****

TASK [Gathering Facts] ****
ok: [control1]
ok: [worker1]
ok: [worker2]

TASK [create the 'ubuntu' user] ****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [allow 'ubuntu' user to have passwordless sudo] ****
changed: [control1]
changed: [worker1]
changed: [worker2]

TASK [set up authorized keys for the ubuntu user] ****
changed: [worker1] => (item=ssh-rsa AAAAB3...)
changed: [worker2] => (item=ssh-rsa AAAAB3...)
changed: [control1] => (item=ssh-rsa AAAAB3...)

PLAY RECAP ****
control1                     : ok=4    changed=3    unreachable=0    failed=0   
worker1                    : ok=4    changed=3    unreachable=0    failed=0   
worker2                    : ok=4    changed=3    unreachable=0    failed=0   
```

Now that the preliminary setup is complete, you can move on to installing Kubernetes-specific dependencies.

\
\
