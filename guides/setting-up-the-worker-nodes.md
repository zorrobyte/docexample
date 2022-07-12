# Setting Up the Worker Nodes

Adding workers to the cluster involves executing a single command on each. This command includes the necessary cluster information, such as the IP address and port of the control plane’s API Server, and a secure token. Only nodes that pass in the secure token will be able join the cluster.

Navigate back to your workspace and create a playbook named `workers.yml`:

```
nano ~/kube-cluster/workers.yml
```

Add the following text to the file to add the workers to the cluster:

```
--- 
- 
  become: true
  gather_facts: false
  hosts: control_plane
  tasks: 
    - 
      name: "get join command"
      register: join_command_raw
      shell: "kubeadm token create --print-join-command"
    - 
      name: "set join command"
      set_fact: 
        join_command: "{{ join_command_raw.stdout_lines[0] }}"
- 
  become: true
  hosts: workers
  tasks: 
    - 
      args: 
        chdir: $HOME
        creates: node_joined.txt
      name: "join cluster"
      shell: "{{ hostvars['control1'].join_command }} >> node_joined.txt"

```

Here’s what the playbook does:

* The first play gets the join command that needs to be run on the worker nodes. This command will be in the following format:`kubeadm join --token <token> <control-plane-ip>:<control-plane-port> --discovery-token-ca-cert-hash sha256:<hash>`. Once it gets the actual command with the proper **token** and **hash** values, the task sets it as a fact so that the next play will be able to access that info.
* The second play has a single task that runs the join command on all worker nodes. On completion of this task, the two worker nodes will be part of the cluster.

Save and close the file when you are finished.

Run the playbook by locally with the following command:

```
ansible-playbook -i hosts ~/kube-cluster/workers.yml
```

On completion, you will see output similar to the following:

```
OutputPLAY [control1] ****

TASK [get join command] ****
changed: [control1]

TASK [set join command] *****
ok: [control1]

PLAY [workers] *****

TASK [Gathering Facts] *****
ok: [worker1]
ok: [worker2]

TASK [join cluster] *****
changed: [worker1]
changed: [worker2]

PLAY RECAP *****
control1                     : ok=2    changed=1    unreachable=0    failed=0   
worker1                    : ok=2    changed=1    unreachable=0    failed=0  
worker2                    : ok=2    changed=1    unreachable=0    failed=0  
```

With the addition of the worker nodes, your cluster is now fully set up and functional, with workers ready to run workloads. Before scheduling applications, let’s verify that the cluster is working as intended.
