---
title: Redoing my DNS setup
description: Documenting my shinanegans in redoing DNS within my lab
---
## Introduction
As I've setup and mainatined my homelab over the years, one of the things I've neglected to "properly" configure was my DNS setup. I've used PiHole in a Docker container on my home server and it was mostly functional. My deployed adlists were blocking ads and malicious domains, and DNS resolutions were fine. However, this setup had some issues, and I was generally not very satisfied with it. 

I had to manually configure local DNS records in the PiHole web UI for my local subdomain of my main domain, as well as the service subdomains of my local subdomain. I disliked this as if I had to add a new service to my homelab, I would need to enter the PiHole web UI and manually add another A record. I used external upstream DNS servers from the likes of Quad9 and Cloudflare, sending my DNS requests to a 3rd party. I had tried to setup an Unbound instance but it didn't work. I also had no redundancy, so if my main server went down, I will get knocks on my door from angry family members. This would force me to configure a 2nd external DNS server in my router in order to avoid that. Overall, it was functional, but it had some big issues. 

This all came to a head when I moved out for college and got a student dorm. I moved in my stuff for the year, and got to work setting up my homelab in my dorm. After a few days of troubleshooting and setup. all my services were operational. It was mostly smooth for a few months. Now that I was out of the house and in an environment where I didn't have to worry about stability, I had finally decided to kick myself and try to fix my DNS setup. I had always wanted to do a setup with PiHole and Unbound in docker containers. This was also around the time I found [Nebula Sync](https://www.youtube.com/watch?v=OcSBggDyeJ4) from Techno Tim. I also happened to have a spare Pi that I was no longer using for my classes. All was looking good and the time was perfect for a DNS redeployment. 

## Initial Setup
So 