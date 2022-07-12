# Creating nodes on DigitalOcean

{% hint style="info" %}
If you don't already have a **DigitalOcean account**, sign up for one [here](https://cloud.digitalocean.com)
{% endhint %}

### Goals

Create three servers running Ubuntu 20.04 with at least 2GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user with your SSH key pair.

1. After logging in, click Create > Droplets

![](<../.gitbook/assets/image (16).png>)

2\. Ensure that Ubuntu 20.04 (LTS) is selected for the OS type, choose plan Basic with 2GB/2GPUs

![](<../.gitbook/assets/image (2).png>)

3\. Select the Datacenter closest to you (or your cutomers) and ensure you select your SSH key

![](<../.gitbook/assets/image (7).png>)

4\. Create 3 Droplets and name them for easy identification later

![](<../.gitbook/assets/image (27).png>)

5\. Click Create Droplet 🎉

#### After a few minutes, your deployments will become available!

![](<../.gitbook/assets/image (12).png>)

{% hint style="info" %}
**Remember:** Dont forget to SSH into each Droplet and accept the host finterprint, it will make things easier later!
{% endhint %}

![](<../.gitbook/assets/image (26).png>)
