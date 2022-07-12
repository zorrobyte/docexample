# Verifying the Cluster

A cluster can sometimes fail during setup because a node is down or network connectivity between the control plane and workers is not working correctly. Let’s verify the cluster and ensure that the nodes are operating correctly.

You will need to check the current state of the cluster from the control plane node to ensure that the nodes are ready. If you disconnected from the control plane node, you can SSH back into it with the following command:

```
ssh ubuntu@control_plane_ip
```

Then execute the following command to get the status of the cluster:

```
kubectl get nodes
```

You will see output similar to the following:

```
OutputNAME     STATUS   ROLES                  AGE     VERSION
control1   Ready    control-plane,master   3m21s   v1.22.0
worker1  Ready    <none>                 32s     v1.22.0
worker2  Ready    <none>                 32s     v1.22.0
```

If all of your nodes have the value `Ready` for `STATUS`, it means that they’re part of the cluster and ready to run workloads.

If, however, a few of the nodes have `NotReady` as the `STATUS`, it could mean that the worker nodes haven’t finished their setup yet. Wait for around five to ten minutes before re-running `kubectl get nodes`and inspecting the new output. If a few nodes still have `NotReady` as the status, you might have to verify and re-run the commands in the previous steps.

Now that your cluster is verified successfully, let’s install KubeCost.
