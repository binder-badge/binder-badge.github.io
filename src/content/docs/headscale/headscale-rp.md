---
title: Setting up Headscale
description: A guide for setting up Headscale behind Traefik
---

## Introduciton
If you used Tailscale, it is a pretty nice way to easily connect all of your devices together in a secure way. Personally, I use it to connect all of my servers together, and to gain access to my homelab without having to port forward for a Wireguard and OpenVPN server. However, this relies on Tailscale's infrastucture, mainly their [control and cooridination server](https://tailscale.com/blog/how-tailscale-works#the-control-plane-key-exchange-and-coordination) in order to provide encryption keys for your clients and for facilitating connections between clients. Now this is fine, but this makes me reliant on Tailscale being alive and not going bankrupt. Whether you are fine with that is up to you, but for me, this makes me interested in hosting my own Tailscale control and cooridination server, which is what [Headscale](https://headscale.net/stable/) aims to do. It is an "open source, self-hosted implementation of the Tailscale control server." This guide will go over how to set this up behind a reverse proxy. 

## Requirements
For this, you will need
- A Linux server
- Ability to port forward ports 80 and 443

## Instrucitons
