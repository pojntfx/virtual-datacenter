# Virtual Datacenter

A virtual Cluster Platform datacenter.

## Overview

This is a virtual Cluster Platform datacenter built on Vagrant.

## Usage

### Kubernetes

#### Setup Builder

```bash
% cd kubernetes/builder
% vagrant up
% k3sserverctl -s :31070 start -f config/k3sserver.yaml
% k3sserverctl -s :31070 get kubeconfig | sed -e "s/6443/36443/g" > ~/.kube/builder-config.yaml
% export KUBECONFIG="${HOME}/.kube/builder-config.yaml"
% kubectl get pod -A
% helm repo add pojntfx https://pojntfx.github.io/charts/
% helm install ipxebuilderd pojntfx/ipxebuilderd --set ingress.domain=ipxebuilderd.virtual-datacenter.local --set 'minio.ingress.hosts[0].name=minio.ipxebuilderd.virtual-datacenter.local'
% echo "127.0.0.1 ipxebuilderd.virtual-datacenter.local" | sudo tee -a /etc/hosts
% ipxectl -s ipxebuilderd.virtual-datacenter.local:30080 apply -f config/ipxe.yaml
% ipxectl -s ipxebuilderd.virtual-datacenter.local:30080 get $ID # Take note of the URL
```

#### Setup Controller

```bash
% cd kubernetes/controller
% vagrant up
% k3sserverctl -s :21070 start -f config/k3sserver.yaml
% k3sserverctl -s :21070 get kubeconfig | sed -e "s/6443/26443/g" > ~/.kube/controller-config.yaml
% export KUBECONFIG="${HOME}/.kube/controller-config.yaml"
% kubectl get pod -A
% helm repo add pojntfx https://pojntfx.github.io/charts/
% helm install dhcpdd pojntfx/dhcpdd --set ingress.domain=dhcpdd.virtual-datacenter.local
% helm install tftpdd pojntfx/tftpdd --set ingress.domain=tftpdd.virtual-datacenter.local
% echo "127.0.0.1 dhcpdd.virtual-datacenter.local tftpdd.virtual-datacenter.local" | sudo tee -a /etc/hosts
% dhcpdctl -s dhcpdd.virtual-datacenter.local:20080 apply -f config/dhcpd.yaml
% tftpdctl -s tftpdd.virtual-datacenter.local:20080 apply -f config/tftpd.yaml --tftpd.biosFilenameURL $URL # URL from above
```

#### Setup Node

```bash
% cd kubernetes/node
% vagrant up
```

### Native

#### Setup Controller

```bash
% cd native/controller
% vagrant up
% dhcpdctl -s :21020 apply -f config/dhcpd.yaml
% tftpdctl -s :21040 apply -f config/tftpd.yaml
```

#### Setup Node

```bash
% cd native/node
% vagrant up
```

## License

Virtual Datacenter (c) 2020 Felicitas Pojtinger

SPDX-License-Identifier: AGPL-3.0
