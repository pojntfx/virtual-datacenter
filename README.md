# Virtual Datacenter

A virtual Cluster Platform datacenter.

## Overview

This is a virtual Cluster Platform datacenter built on Vagrant.

## Usage

### Kubernetes

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
% tftpdctl -s tftpdd.virtual-datacenter.local:20080 apply -f config/tftpd.yaml
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
