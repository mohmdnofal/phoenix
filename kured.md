
# Introduction

Azure automatically applies security patches to the nodes in your AKS cluster on a nightly schedule. However, you are *responsible for ensuring that nodes are rebooted as required*. You have several options for performing node reboots: 

* Manually, through the Azure portal or the Azure CLI. 

* By upgrading your AKS cluster. Cluster upgrades automatically cordon and drain nodes, then bring them back up with the latest Ubuntu image. You can use this process without changing Kubernetes versions by performing an az aks upgrade targeting the same Kubernetes version currently on your cluster. 

* Using Kured, an open-source reboot daemon for Kubernetes. Kured runs as a DaemonSet and monitors each node for the presence of a file indicating that a reboot is required. It then orchestrates those reboots across the cluster, following the same cordon and drain process described earlier. 

In this challaenge we will focus on Kured in order to automate the reboot process.



# Kured 
https://github.com/weaveworks/kured

Kured (KUbernetes REboot Daemon) is a Kubernetes daemonset that performs safe automatic node reboots when the need to do so is indicated by the package management system of the underlying OS.

* Watches for the presence of a reboot sentinel e.g. /var/run/reboot-required
* Utilises a lock in the API server to ensure only one node reboots at a time
* Optionally defers reboots in the presence of active Prometheus alerts
* Cordons & drains worker nodes before reboot, uncordoning them after



