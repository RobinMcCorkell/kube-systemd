Kubernetes under systemd
========================

Prerequisites
-------------

- `systemd-docker`: https://github.com/ibuildthecloud/systemd-docker
- Docker
- `cfssl`

Generate SSL certs
------------------

- /etc/kubernetes/ssl contains certificates for the apiserver, needed by components
  which talk to Kubernetes.
- /etc/kubernetes/sslkubelet contains certificates for the kubelet, needed by
  components which schedule Kubernetes pods (usually just the apiserver).
- /root/ssl (or whatever you rewrite this to) contains certificates for etcd.
- /etc/kubernetes/serviceaccount contains an RSA key (not a certificate) for
  service account token signing in Kubernetes.

Start etcd
----------

`systemctl start etcd`

Starts an etcd server with HTTPS on port 2379, and on localhost:2378

Configure flannel
-----------------

Change settings in /etc/kubernetes/flannel.json if necessary.

`curl -sSL http://localhost:2379/v2/keys/coreos.com/network/config -XPUT -d value="$(< /etc/kubernetes/flannel.json)"`

This only needs to be run once, since the data is stored in etcd.

Start flannel
-------------

`systemctl start flannel`

Configure kubernetes
--------------------

See /etc/conf.d/kubernetes, and /etc/kubernetes/manifests

Start kubernetes
----------------

`systemctl start kubernetes`

???
---

PROFIT.
