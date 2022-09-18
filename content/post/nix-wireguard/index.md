---
title: NixOS WireGuard
description: A short guide about setting up your own WireGuard connection!
slug: nixos-wireguard
date: 2021-06-16 10:22:12+0000
---

# `Wireguard.nix`
One of the many free & open-source VPN protocol and when fused with the
wonderful declarative Nix configuration one is able to setup their system
without hinders!

In this post I will attempt to explain how to setup a declarative
`wireguard.nix` module, enjoy!

> **Note** 
> The `<<X>>` provided in the source-blocks links the remaining source blocks
> with the source-block they are input in. This is useful when writing your Nix
> configurations in org-mode and later tangling (outputs the code only) the
> result to a `example.nix` file.


## Systemd.service: `wg-quick-wg0`.
This is not a necessary step, but to prevent the service from starting during
boot while remaining enabled one has to specify the ~wantedBy~ option.

Another reason to why I am using the following setup is to add the ~path~,
listed in the ~systemd.services~, to the scope which helps me reduce the amount
of references required when enabling the kill-switch later on.

```nix
{ config, pkgs, lib, ... }:
{
  systemd.services."wg-quick-wg0" = {
    requires = [ "network-online.target" ];
    after = [ "network.target" "network-online.target" ];
    wantedBy = lib.mkForce [ ];
    environment.DEVICE = "wg0";
    path = [ pkgs.wireguard-tools pkgs.iptables pkgs.iproute ];
 };

  <<networking>>
}
```

> **Note** 
> You are not limited to using ~wg0~ as an interface name. You could instead
> use whatever name you deem fit for your `wg-quick` interface!

## Networking: Setup your `wg-quick` Interface
Define your `wg-quick` interface (make sure to replace `wg0` with your
interface name) and obviously fill in the missing info inside the quotes:
```nix
  networking = {
    wg-quick.interfaces = {
      wg0 = {
        address = [ "" ];
        dns = [ "" ];
        listenPort = 51820;
        privateKeyFile = "wireguard_private.key";

        peers = [{
          publicKey = "";
          allowedIPs = [ "" ];
          endpoint = "";
          persistentKeepalive = 25;
        }];

        <<kill-switch>>
      };
    };
  };
```

### Kill-Switch: Prevent DNS Leakage!
When the connection is probable to leaking your DNS, the connection will
completely shutdown and prevent any form of network flow unless the
systemd-service has been restarted:

```nix
postUp = ''
  iptables -I OUTPUT ! -o wg0 -m mark ! --mark $(wg show wg0 fwmark) -m addrtype ! --dst-type LOCAL -j REJECT  && ip6tables -I OUTPUT ! -o wg0 -m mark ! --mark $(wg show wg0 fwmark) -m addrtype ! --dst-type LOCAL -j REJECT
'';

preDown = ''
  iptables -D OUTPUT ! -o wg0 -m mark ! --mark $(wg show wg0 fwmark) -m addrtype ! --dst-type LOCAL -j REJECT && ip6tables -D OUTPUT ! -o wg0 -m mark ! --mark $(wg show wg0 fwmark) -m addrtype ! --dst-type LOCAL -j REJECT
'';
```

# Congratulations!
You now have a fully functional `wg-quick` interface!
