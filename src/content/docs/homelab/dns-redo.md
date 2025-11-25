---
title: Redoing my DNS setup
description: Documenting my shinanegans in redoing DNS within my lab
---
## Introduction
As I've setup and mainatined my homelab over the years, one of the things I've neglected to "properly" configure was my DNS setup. I've used PiHole in a Docker container on my home server and it was mostly functional. My deployed adlists were blocking ads and malicious domains, and DNS resolutions were fine. The web UI was behind my Traefik reverse proxy with a cool dedicated subdomain under my local subdomain tha I used for my server. However, this setup had some issues, and I was generally not very satisfied with it. 

I had to manually configure local DNS records in the PiHole web UI for my local subdomain of my main domain, as well as the service subdomains of my local subdomain. I disliked this as if I had to add a new service to my homelab, I would need to enter the PiHole web UI and manually add another A record. I used external upstream DNS servers from the likes of Quad9 and Cloudflare, sending my DNS requests to a 3rd party. I had tried to setup an Unbound instance but it didn't work. My intial plan to have PiHole and Unbound as seperate Docker stacks didn't pan out well as I had to create external Docker networks in order for them to communicate with each other. It was also a pain to make changes. I also had no redundancy, so if my main server went down, I will get knocks on my door from angry family members. This would force me to configure a 2nd external DNS server in my router in order to avoid that. Overall, it was functional, but it had some big issues. 

This all came to a head when I moved out for college and got a student dorm. I moved in my stuff for the year, and got to work setting up my homelab in my dorm. After a few days of troubleshooting and setup. all my services were operational. It was mostly smooth for a few months. Now that I was out of the house and in an environment where I didn't have to worry about stability, I had finally decided to kick myself and try to fix my DNS setup. I had always wanted to do a setup with PiHole and Unbound in docker containers. This was also around the time I found [Nebula Sync](https://www.youtube.com/watch?v=OcSBggDyeJ4) from Techno Tim. I also happened to have a spare Pi that I was no longer using for my classes. All was looking good and the time was perfect for a DNS redeployment. 

## Initial Setup
Once the assignments from my courses started lightening up (or randomly became productive), I started work on a new Docker stack, where I would consolidate my DNS setup into a single Docker Compose file. 

It would consist of 3 parts:
- PiHole as the main "frontend" DNS provider
- Unbound as PiHole's upstream DNS as well as root DNS server
- Nebula Sync for syncing up my ad lists (and potentially other settings) to my Pi, running a similar PiHole + Unbound setup. 

For initial testing and iteration, I would base my PiHole setup off the base PiHole compose file from PiHole themselves, and then add on an Unbound configuarion on top. The syncing would come later once I confirmed that Unbound and PiHole worked well together on both my main server and Pi. 
Below is an approximation to my first variation of my Compose file. 
```yaml
services:
  pihole:
    container_name: pihole
    hostname: pihole
    image: pihole/pihole:latest
    networks:
      proxy: null
      dns:
        ipv4_address: 10.0.0.3
    ports:
      - 54:53/tcp
      - 54:53/udp
      - 201:80/tcp
    environment:
      TZ: America/New_York
      FTLCONF_webserver_api_password: old-pass
      FTLCONF_dns_listeningMode: all
    volumes:
      - ./pihole:/etc/pihole
      - ./dnsmasq.d:/etc/dnsmasq.d
    cap_add:
      - NET_ADMIN 
      - SYS_NICE
    depends_on:
      - unbound
    restart: unless-stopped
  unbound:
    hostname: unbound
    container_name: unbound
    image: mvance/unbound:latest
    networks:
      dns:
        ipv4_address: 10.0.0.2
    volumes:
      - ./unbound:/opt/unbound/etc/unbound
    healthcheck:
      test:
        - NONE
    restart: unless-stopped
networks:
  dns:
    driver: bridge
    ipam:
      config:
        - subnet: 10.0.0.0/16
          gateway: 10.0.0.1
```
I would also use the [example configuration](https://github.com/MatthewVance/unbound-docker/blob/master/unbound.conf) for my Unbound from the [mvance/unbound](https://github.com/MatthewVance/unbound-docker) repository. 

This version of my stack would allow me to get basic instances of PiHole and Unbound up and running via a single Compose file. I didn't even bother putting it behind Traefik as I didn't want other factors potentially messing things up. I would test DNS resolution by running dig commands on my main laptop or on the server itself via the PiHole container if I was testing Unbound's DNS resolution. I would test popular domains like `google.com`, my own domain and its subdomains. One issue I found was that my local subdomain wasn't returning any IP address. This was because that subdomain lead to an IP that was classified as a "private IP" in Unbound. 

Now to solve this issue, I can do this in a few ways. I can do the resolution for this subdomain either at "frontend" DNS via PiHole, or at the root DNS level via Unbound. If I wanted to do it at the PiHole level, I had 2 options. The first was my old way of doing things, which was to manually create an A record for my local subdomain and for every service subdomain of my local subdomain, which was what I was trying to avoid for the reasons I mentioned earlier. The 2nd option was to add 