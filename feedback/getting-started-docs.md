# Getting Started Docs

The higher quality our documentation, the less customers will need to reach out for help. This not only reduces our case load, but also helps us scale as we gain new customers.

* Is it not clear if Helm should be installed on the control plane node or the user's local machine. I spun up my own k8s cluster on DO and had erroneously installed it to the control plane node. Such could be figured out when troubleshooting, but a quick clarification could be helpful to save some time for the customer, especially if they are new to Helm

![](<../.gitbook/assets/image (1).png>)

*   My custom cluster did not have a _PersistentVolume or PVC_ configured and manually configuring a PVC was time consuming. We can assume that a customer running k8s in production already has a PVC, but I did feel a little lost when instaling KubeCost and the pod wouldn't start.&#x20;

    ```
    error: unable to forward port because pod is not running. Current status=Pending
    ```

It could be helpful to add a note in the [Getting Started doc](https://www.kubecost.com/install.html#show-instructions) under Step 2: Enable port-forward that a PVC is required

&#x20;[https://guide.kubecost.com/hc/en-us/articles/4407601830679#a-name-no-cluster-a-issue-unable-to-connect-to-a-cluster](https://guide.kubecost.com/hc/en-us/articles/4407601830679#a-name-no-cluster-a-issue-unable-to-connect-to-a-cluster)

After succesfully port forwarding, I was greeted with `No Clusters Found` error

![](<../.gitbook/assets/image (4).png>)

We should add a note that it could take a moment for KubeCost to be ready in the getting started guide and the troubleshooting guide: [https://guide.kubecost.com/hc/en-us/articles/4407601830679#a-name-no-cluster-a-issue-unable-to-connect-to-a-cluster](https://guide.kubecost.com/hc/en-us/articles/4407601830679#a-name-no-cluster-a-issue-unable-to-connect-to-a-cluster)

* It would be nice of us to link how to publish a secure endpoint `Publish :9090 as a secure endpoint on your cluster to remove the need to port forward.`
* It could be nice to offer a single line installer script like NewRelic offers
