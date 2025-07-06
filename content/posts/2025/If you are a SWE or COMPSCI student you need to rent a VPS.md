+++
title = "If You Are a SWE or COMPSCI Student You Need to Rent a VPS"
date = "2025-07-06T15:23:24-06:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
# tags = ["vps", "server"]
keywords = ["", ""]
description = "If you are a COMPSCI/SWE studen, renting a vps is an amazing learning experience."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

# Renting a VPS

For whatever reason I decided to finally quit procrastronatinating and rent a VPS. I learnt so much just from doing so that I think every student should rent a VPS, and they should do it atleast like one year into learning programming.

## Over-Abstractions

Using *PaaS*'s like railway or vercel are very convinient and very quick to get from nothing to a application but as a student we are robbing ourselves by using them. We can't fully appreciate what these services do if went can't dont
know how to get our hands dirty ssh into machine and configure server.

Here is what one could learn setting up a simple flask app in a VPS.

## Before renting a VPS

Obviously if you rent a VPS it better be some flavor of linux. So if you dont already have experience with a linux distro I recommend debian.

One would also have to know the basic shell commands. You dont have to be a shell wizard but you must know how to get around.

## Renting a VPS

Once you have a VPS its recommended you do hardening for security purposes.

- Hardening sshd by not disabling password authentication
  - Generating ssh keys.
  - Create a user and disabling login as root.
  - Disable default ssh port (I dont recomend this because its security through security).

(this is just some cool trivia. This is the email correspondence between the creator of SSH and IANA for ssh being assigned port 22)

> From ylo Mon Jul 10 11:45:48 +0300 1995 From: Tatu Ylonen <ylo@cs.hut.fi>
>
> To: Internet Assigned Numbers Authority <iana@isi.edu>
>
> Subject: request for port number
>
> Organization: Helsinki University of Technology, Finland
>
> Dear Sir, I have written a program to securely log from one machine into another over an insecure network. It provides major improvements in security and functionality over existing telnet and rlogin protocols and implementations. In particular, it prevents IP, DNS and outing spoofing.  My plan is to distribute the software freely on the Internet and to get it into as wide use as possible. I would like to get a registered privileged port number for the software.
>
> The number should preferably be in the range 1-255 so that it can be used in the WKS field in name servers. I'll enclose the draft RFC for the protocol below. The software has been in local use for several months, and is ready for publication except for the port number. If the port number assignment can be arranged in time, I'd like to publish the software already this week. I am currently using port number 22 in the beta test.
>
> It would be great if this number could be used (it is currently shown as Unassigned in the lists). The service name for the software is "ssh" (for Secure Shell).
>
> Yours sincerely,  Tatu Ylonen <ylo@cs.hut.fi>  ... followed by protocol specification
> for ssh-1.0

The next day, I had an e-mail from Joyce waiting in my mailbox:

> Date: Mon, 10 Jul 1995 15:35:33 -0700 From: jkrey@ISI.EDU To: ylo@cs.hut.fi
> Subject:
> Re: request for port number  Cc: iana@ISI.EDU
>
> Tatu,  We have assigned port number 22 to ssh, with you as the point of contact.
> Joyce

(so kool)

To edit the config files its not as if you can just open your
favorite text editor and get to work. You must use what ever come's installed in your distro of choice, nano and vim are installed in about every system so I recommend you learn one of these.

## What now?

Now that you have done some very basic security you can host whatever services which you like. There is all kinds of things that you can do.

- Minecraft server to play with friends.
- A forward proxy to get around your schools firewall (i haven't tried this yet but I will soon).
- Host services that can be used for personal projects in portfolio.
- Host a hidden service on tor network (so scawy O_O).

The main reason I decided to rent a VPS was for hosting projects that I could not host for free on github pages.

I learned so much just from hosting a flask app I made in a weekend.

The quickest way to get started with a flask app is to run

```bash
flask run --host=0.0.0.0 --port=<port of your choosing>
```

I personally did not know that the IP "0.0.0.0" was some special port like "127.0.0.1". The "0.0.0.0" means that it will listen to all network interfaces.

This "works" and I could access the application by going into my browser and copy pasting my VPS ip and port.

The second step is to forget about copy pasting or remembering what is the IP of your vps, dich that and buy a domain name if you dont already have one. Once you have 'bought' a domain name you set a records pointing to your VPS.

Now I personally had already had my domain name ([jorge.sh](https://jorge.sh)), but it was pointing to github pages. This means I just have to setup a subdomain for my vps.

You have to be a little patient for the record to propagate through the DNS system but was thats done you are still not done.

Having HTTPS on your app is a must. The most common way to set up HTTPS is by utilizing a web server as a reverse proxy. I decided to go with caddy because it seemed like the most plug and play experience over NGINX.

After you installed your webserver of choice, you have to stop the flask application you are running. Run it again but this time with these flags.

```
flask run --host=127.0.0.1 --port=<port of your choosing>
```

Now you go back and edit your webservers config to serve as a reverse proxy to your flask app but wait your still not done. Flask explicitly tells you not to use ```flask run``` in production. So you then install a WSGI server and have your webserver redirect traffic to that instead.

## What did we learn?

Its really amazing, I did not expect to learn this much just by hosting some applications for my portfolio.

I would have never seen this if I had decided to just push the code into a github repository and call it a day or use some service like railway.

Yet there is still so much more that I can do.

As you can see the system is not very reproducible, If I for some reason or other decide to move VPS, well I would have to install and reconfigure all the applications from scratch again.

Another pain is CD/CI. Any changes I decide to make in my application, how do I do that?

Do I develop on my machine and once im done `scp` the project over, then stop the service `rm -rf` the old project and run the new project. Or do I edit the code directly in the VPS?

These are solutions that wont scale, but thats ok, as you learn more you increase the complexity as you go.

## Next step

The next step is to learn Docker and Kubernetes but I decided not to learn those yet. How much would I gain making complex pipelines when I have 0 users?
