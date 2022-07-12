# Installing KubeCost

### Before you begin

In order to deploy the Kubecost helm chart you'll need to install Helm 3 on your local machine:

****[**Install Helm**](https://helm.sh/docs/intro/install/)****

Next, you'll need to configure a [`PersistentVolumeClaim`](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) (PVC) as part of your [deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/). The claim can allow cluster workers to read and write database records, user-generated website content, log files, and other data that should persist after a process has completed. This is required for KubeCost to function correctly.

{% embed url="https://docs.digitalocean.com/products/kubernetes/how-to/add-volumes" %}

{% hint style="warning" %}
Note that manually configuring PVCs is time consuming and highly varies per use case so I've omitted the specific steps for this homework assignment
{% endhint %}

### Step 1: Install Kubecost

Running the following commands will also install Prometheus, Grafana, and kube-state-metrics in the namespace supplied. View install configuration options [here](https://github.com/kubecost/cost-analyzer-helm-chart/blob/master/README.md#config-options).

**helm 3**

```
kubectl create namespace kubecost 
helm repo add kubecost https://kubecost.github.io/cost-analyzer/ 
helm install kubecost kubecost/cost-analyzer --namespace kubecost --set kubecostToken="em9ycm9ieXRlQGdtYWlsLmNvbQ==xm343yadf98"
```

### Step 2: Enable port-forward

```
kubectl port-forward --namespace kubecost deployment/kubecost-cost-analyzer 9090
```

Having installation issues? View our [Troubleshooting Guide](http://docs.kubecost.com/troubleshoot-install) or contact us directly at team@kubecost.com.

### Step 3: See the data! 🎉

You can now view the deployed frontend by visiting the following link. Publish :9090 as a secure endpoint on your cluster to remove the need to port forward.

```
http://localhost:9090
```

{% hint style="warning" %}
Note that if you receive a No Clusters Found, Cluster not available error, wait a few minutes and try again. KubeCost can take a moment to start
{% endhint %}

![](<../.gitbook/assets/image (23).png>)

![](<../.gitbook/assets/image (14).png>)

With this newfound visibility, teams often start with&#x20;

1. Looking at cost allocation trends&#x20;
2. Searching for quick cost savings or reliability improvements. View our [Getting Started](http://docs.kubecost.com/#getting-started) guide for more information on product configuration and common initial actions.

### Bonus

How many of these health checks can you resolve?

![](<../.gitbook/assets/image (15).png>)

### Updating KubeCost

Now that your Kubecost chart is installed, you can update with the following commands:

**helm 2**

```
helm repo update && helm upgrade kubecost kubecost/cost-analyzer file_copy
```

**helm 3**

```
helm repo update && helm upgrade kubecost kubecost/cost-analyzer -n kubecost
```

### Deleting KubeCost

To uninstall Kubecost and its dependencies, run the following command:

**helm 2**

```
helm del --purge kubecost file_copy
```

**helm 3**

```
helm uninstall kubecost -n kubecost
```

We're available any time for questions or concerns at [team@kubecost.com](mailto:team@kubecost.com) and Slack ([invite](https://join.slack.com/t/kubecost/shared\_invite/enQtNTA2MjQ1NDUyODE5LWFjYzIzNWE4MDkzMmUyZGU4NjkwMzMyMjIyM2E0NGNmYjExZjBiNjk1YzY5ZDI0ZTNhZDg4NjlkMGRkYzFlZTU)).
