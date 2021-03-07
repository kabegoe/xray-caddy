# Xray-Caddy in Docker

## Enable IPv6 in Docker by NAT

### Genreal guide
https://valinux.hatenablog.com/entry/20200709

### Step1: Enable IPv6 on bridge in Docker network
Edit Docker daemon config and restart Docker daemon service. See https://docs.docker.com/config/daemon/ipv6/ for more details.

```
{
  "ipv6": true,
  "fixed-cidr-v6": "fd86::/64"
}
```

### Step2: Enable IPv6 NAT on host

#### Option1: nftable
See https://wiki.nftables.org/wiki-nftables/index.php/Main_Page

**Install nftable**

See https://wiki.debian.org/nftables
```
$ sudo apt install nftable
```

**Configure and enable nftable setting**

See

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/getting-started-with-nftables_configuring-and-managing-networking

https://wiki.archlinux.org/index.php/nftables

https://tokyodebian-team.pages.debian.net/pdf2019/debianmeetingresume201912.pdf

http://forums.debian.net/viewtopic.php?f=16&t=143876


**Add Masquerading rule for IPv6**

```
$ sudo nft add table ip6 nat
$ sudo nft add chain ip6 nat POSTROUTING { type nat hook postrouting priority 100 \; }
$ sudo nft add rule ip6 nat POSTROUTING oifname != "docker0" ip6 saddr fd86::/64 counter masquerade
```

#### Option2: ufw
**Add following snippet to `/etc/ufw/before6.rules`.**

```
$ sudo vim /etc/ufw/before6.rules

/etc/ufw/before6.rules
...
# Docker IPv6 port forward
*nat
:POSTROUTING ACCEPT [0:0]
-A POSTROUTING -s fd86::/64 ! -o docker0 -j MASQUERADE
COMMIT
...
*filter
...
```
