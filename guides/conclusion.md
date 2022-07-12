# Conclusion

In this guide, you’ve successfully set up a Kubernetes cluster on Ubuntu 20.04 using Kubeadm and Ansible for automation, we also installed KubeCost for cost monitoring and resource optimization.

If you’re wondering what to do with the cluster now that it’s set up, a good next step would be to get comfortable deploying your own applications and services onto the cluster. Here’s a list of links with further information that can guide you in the process:

* [Dockerizing applications](https://docs.docker.com/engine/examples/) - lists examples that detail how to containerize applications using Docker.
* [Pod Overview](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/) - describes in detail how Pods work and their relationship with other Kubernetes objects. Pods are ubiquitous in Kubernetes, so understanding them will facilitate your work.
* [Deployments Overview](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) - provides an overview of deployments. It is useful to understand how controllers such as deployments work since they are used frequently in stateless applications for scaling and the automated healing of unhealthy applications.
* [Services Overview](https://kubernetes.io/docs/concepts/services-networking/service/) - covers services, another frequently used object in Kubernetes clusters. Understanding the types of services and the options they have is essential for running both stateless and stateful applications.
* [KubeCost Documentation](https://guide.kubecost.com/hc/en-us/articles/4407595947799-Getting-Started) - optimize your resource usage and spend on your Kubernetes cluster so you can invest more into your application development instead of infrastructure costs.

Other important concepts that you can look into are [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/), [Ingresses](https://kubernetes.io/docs/concepts/services-networking/ingress/) and [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/), all of which come in handy when deploying production applications.

Kubernetes has a lot of functionality and features to offer. [The Kubernetes Official Documentation](https://kubernetes.io/docs/) is the best place to learn about concepts, find task-specific guides, and look up API references for various objects. You can also review our [Kubernetes for Full-Stack Developers curriculum](https://www.digitalocean.com/community/curriculums/kubernetes-for-full-stack-developers).
