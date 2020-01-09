[Reddit Thread](https://www.reddit.com/r/kubernetes/comments/be0415/k3s_minikube_or_microk8s/)


> Hey all,


>So I just started messing around with K3s, and I really like how simple it is to get up and running. But I am not knowledgeable enough to fully grok what the difference is with K3s, minikube, microk8s and even the full k8s.

>My situation is I want to learn/use Kubernetes in production, but be able to learn it locally as I dont have the coin to pay for cloud at this point. More so, I have a few ideas I want to build into k8 deployments as if they were going in to production, but try them out on my local laptop/desktop dev setup... yet ensure that what I run locally will be identical (without the scaling capability) as what would run in production. In other words, I want to learn/play with/configure an ingress smart router/LB/API gateway of some sort (leaning towards Ambassador I think if I can figure out if it can handle all my API RBAC stuff), multiple microservices, databases, and so forth.

>From what I read about K3s, it is basically a full blown K8 implementation without some soon to be removed deprecated bits, and cloud native specific bits.. which I dont know if that means there was some GKE/AKS/etc bits in the K8 code that was taken out or what? More so, if that is what it means, does that in any way affect my ability to ensure what I deploy/test on a K3s local distribution will work the same way in a K8 production like GKE? What is missing when I go to deploy to GKE or Digital Ocean Kubernetes?

>I also really like.. if I understand it correctly, that I can set up a few of my machines around the house that are not used often, with nodes that register with my main K3s dev machine.. and it could.. again.. if I understand it correctly.. use those different machines to deploy nodes/containers to.. or maybe it is entire clusters per machine I register?



>Anyway, would appreciate any info on the subject.. I was using minikube, but K3s just strikes me as the better way to go at this point, and it is so much easier to install and use and I can add more hardware, etc.

>Thanks.
--------------------------------------------------------------------------------------------------------------------------------
As you may already know, K3s, minikube, and microk8s are different ways to spin up kubernetes clusters. Each of these solutions achieve their intended purposes with different goals in mind and have their own set of trade-offs.

## Minikube

Minikube can run on Windows and MacOS, because it relies on virtualization (e.g. Virtualbox) to deploy a kubernetes cluster in a Linux VM. You can also run minikube directly on linux with or without virtualization. It also has some developer-friendly features, like [add-ons](https://github.com/kubernetes/minikube/blob/master/docs/addons.md).

Minikube is currently limited to a single-node Kubernetes cluster (for details, see this [issue](https://github.com/kubernetes/minikube/issues/94)). Although, it is on their roadmap.

## Microk8s

_Disclaimer: of all the K8s offerings, I know the least about this one_

Microk8s is similar to minikube in that it spins up a single-node Kubernetes cluster with its own set of [add-ons](https://github.com/ubuntu/microk8s#list-of-available-addons).

Like minikube, microk8s is limited to a single-node Kubernetes cluster, with the added limitation of only running on Linux and only on Linux where `snap` is installed.

## K3s

K3s runs on any Linux distribution without any additional external dependencies or tools. It is marketed by Rancher as a lightweight Kubernetes offering suitable for edge environments, IoT devices, CI pipelines, and even ARM devices, like Raspberry Pi's. K3s achieves its lightweight goal by stripping a bunch of features out of the Kubernetes binaries (e.g. legacy, alpha, and cloud-provider-specific features), replacing docker with containerd, and using sqlite3 as the default DB (instead of etcd). As a result, this lightweight Kubernetes only consumes 512 MB of RAM and 200 MB of disk space. K3s has some nice features, like Helm Chart support out-of-the-box.

Unlike the previous two offerings, K3s can do multiple node Kubernetes cluster. However, due to technical limitations of SQLite, K3s currently does not support High Availability (HA), as in running multiple master nodes. The K3s team plans to address this in the future.

Now, on to some honorary mentions...

## Kind

Kind (Kubernetes-in-Docker), as the name implies, runs Kubernetes clusters in Docker containers. This is the official tool used by Kubernetes maintainers for Kubernetes v1.11+ conformance testing. It supports multi-node clusters as well as HA clusters. Because it runs K8s in Docker, kind can run on Windows, Mac, and Linux.

Kind is optimized first and foremost for CI pipelines, so it may not have some of the developer-friendly features of other offerings.

## Desktop Docker

Docker for Mac/Windows now ships with a bundled Kubernetes offering.

However:
- Kubernetes versions are tightly coupled with the Docker version (i.e. Docker stable channel ships with K8s v1.10. If you want K8s v1.13, you need to switch to Docker edge channel).
- Not as easy to destroy and start a new K8s cluster. AFAIK, you would have to disable Kubernetes and re-enable it through the Docker desktop app preferences.

## K3d

A new project that aims to bring K3s-in-Docker (similar to kind).

They're still working through some issues, like exposing the right docker ports ...etc.

## Kubeadm

The official CNCF tool for provisioning Kubernetes clusters in a variety of shapes and forms (e.g. single-node, multi-node, HA, self-hosted)

Although this is the most manual way to create and manage a cluster of all the offerings listed here.

... And there are plenty more that I do not remember off the top of my head.

---


So, it all depends on what you want to get out these tools and out of Kubernetes.

Are you a developer who wants a simple K8s cluster and don't need or care about multi-node/HA features?

Are you a developer working on a distributed application that runs on Kubernetes and need to test various failure scenarios (e.g. node failure)?   

Are you trying to learn about Kubernetes from a cluster administrator's perspective?

I hope you find this information helpful.

Let me know if you have any questions.

-petermbenjamin

------------------------------------------------------------------------------------------------------------------------
## More:
[Go Labs](https://github.com/mkrupczak3/gopherlabs)


[K8s tools](https://github.com/mkrupczak3/K8S-tools-link)
