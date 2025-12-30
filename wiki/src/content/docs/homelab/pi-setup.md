---
title: Baking a Pi in my lab
description: An article about integrating a Pi into my lab
---
## Background 
Since I've started tinkering around with my homelab and teaching myself server adminstration and networking during high school, I've always stuck with a single machine in my homelab. While that single machine served my needs pretty well, the stability of my simple lab hung on in the hands of my main server. I've always disliked that. I wanted my lab to be reliable, as well as a small little playground for me to tinker around with new services and to mess around with networking. 

However, living with my parents gave me some restrictions with what I could do with my lab. The physical space that I had for my lab was relatively small, as it was a small shelf in my parents' bedroom, that also stored the router along with other things. I also had the unoffical role as the network administrator of the house, as I was the most technically knowledgable in the house. This would mean that if something was wrong with the family's internet connection, or someone had an issue with their machine, I would usually hear my name along with every IT's favorite phrases of "My WiFi isn't working!" or "Something is wrong with my computer!" This would force me to essentially do things in order to prevent instability and I couldn't things things that could potentially break their internet. 

Fast forward to me moving out of the house and bringing my homelab with me to college, I now had my own space. I also bought a Raspberry Pi 3B+ kit as that was one of the things needed for one of my classes. I had only used the Pi once for that class, and never again. Now that I had a 2nd computer that I could use as a 2nd server, I realized that I can finally add a 2nd machine into my lab, and make it more robust and redundant. 

## Well, what now? 
Now that a 2nd server was in my hands, I started thinking up for some potential uses for using this thing in my lab. I mainly wanted to have redundancy for certain important services in order to keep some level of functionality in case of my main server going down. I also had to consider that this was a relatively weak single board, so I didn't have plans to run many things on it. 

I had also started experimenting with Raspian and Ubuntu for the Pi. I was considering Alpine, but I didn't want to do a lot of work to get stuff working. After some deploying the various operating systems, and I had settled on Raspbian. This was because it was already something that I am using and was familiar with. It was also a bit lighther than Ubuntu. 

Now that the OS was settled on, I now had to decide what services this should run. On my main server, I had a variety of services available. I had 
- uptime monitoring
- DNS ad-blocking
- music streaming
- podcast streaming
- 3rd party viewers for social media like Twitter and Reddit
- video and picture conversion tools
- PDF editing tools
- a search engine
- and many many more services. 

