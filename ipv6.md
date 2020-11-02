# About nftable
See https://wiki.nftables.org/wiki-nftables/index.php/Main_Page

# Install nftable
See https://wiki.debian.org/nftables
```
sudo apt install nftable
```

# Configure and enable nftable setting
See
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/getting-started-with-nftables_configuring-and-managing-networking
https://wiki.archlinux.org/index.php/nftables
https://tokyodebian-team.pages.debian.net/pdf2019/debianmeetingresume201912.pdf
http://forums.debian.net/viewtopic.php?f=16&t=143876

# Edit Docker daemon config to enable IPv6 on bridge
# & Restart Docker daemon service
See https://docs.docker.com/config/daemon/ipv6/
```
{
  "ipv6": true,
  "fixed-cidr-v6": "fd86::/64"
}
```

# Add Masquerading rule for IPv6

```
sudo nft add table ip6 nat
sudo nft add chain ip6 nat POSTROUTING { type nat hook postrouting priority 100 \; }
sudo nft add rule ip6 nat POSTROUTING oifname != "docker0" ip6 saddr fd86::/64 counter masquerade
```
