---
title: Configuring RancherOS Networking
layout: os-default

---

## Configuring RancherOS Networking
---

There are two ways to configure networking on RancherOS.

You can change the networking settings by using `ros config` to set different keys within the network key. Anything set using this command will have its change saved in the `rancher.yml` file. Changes will only take affect after you reboot. To learn more information about configuring the networking settings by using `ros config`, please refer to our [`ros config`]({{site.baseurl}}/os/rancheros-tools/ros/config) docs. 

Alternatively, you can use a [cloud config]({{site.baseurl}}/os/cloud-config) file to set up how the network is configured. Cloud config is applied to the RancherOS instance when RancherOS starts.

We'll provide some examples using both the `ros config` or setting it through the cloud config file.

### DNS

Using `ros config`, you can set the `nameserver`, `search`, and `domain`, which directly map to the fields of the same name in `/etc/resolv.conf`.

```bash
$ sudo ros config set network.dns.domain myexampledomain.com
$ sudo ros config get network.dns.domain
myexampledomain.com
```

If you wanted to configure the DNS through the cloud config file, you'll need to place DNS configurations within the `rancher` key.

```yaml
#cloud-config

#Remember, any changes for rancher will be within the rancher key
rancher:
  network:
    dns:
      domain: myexampledomain.com
```

### Interfaces

Using `ros config`, you can configure specific interfaces. Wildcard globbing is supported so `eth*` will match `eth1` and `eth2`.  The available options you can configure are `address`, `gateway`, `mtu`, and `dhcp`.

```bash
$ sudo ros config set network.interfaces.eth1.address 172.68.1.100/24
$ sudo ros config set network.interfaces.eth1.gateway 172.68.1.1
$ sudo ros config set network.interfaces.eth1.mtu 1500
$ sudo ros config set network.interfaces.eth1.dhcp false
```

If you wanted to configure the interfaces through the cloud config file, you'll need to place interface configurations within the `rancher` key.

```yaml
#cloud-config

#Remember, any changes for rancher will be within the rancher key
rancher:
  network:
    interfaces:
      eth1:
        address: 172.68.1.100/24
        gateway: 172.68.1.1
        mtu: 1500
        dhcp: false
```

### Multiple NICs

If you want to configure one of multiple network interfaces, you can specify the MAC address of the interface you want to configure.

Using `ros config`, you can specify the MAC address of the NIC you want to configure as follows:

```bash
$ sudo ros config set network.interfaces.”mac=ea:34:71:66:90:12:01”.dhcp true
```

Alternatively, you can place the MAC address selection in your cloud config file as follows:

```yaml
#cloud-config

#Remember, any changes for rancher will be within the rancher key
rancher:
  network:
    interfaces:
      "mac=ea:34:71:66:90:12:01":
         dhcp: true
```



