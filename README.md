# Virtual Datacenter

A virtual Cluster Platform datacenter.

## Overview

This is a virtual Cluster Platform datacenter built on Vagrant.

## Usage

### Setup Controller

```bash
% cd native/controller
% vagrant up
% dhcpdctl -s localhost:21020 apply -f config/dhcpd.yaml
% tftpdctl -s localhost:21040 apply -f config/tftpd.yaml
```

### Setup Node

```bash
% cd native/node
% vagrant up
```

## License

Virtual Datacenter (c) 2020 Felicitas Pojtinger

SPDX-License-Identifier: AGPL-3.0
