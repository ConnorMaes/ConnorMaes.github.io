# How To Become Anonymous

## A noob's guide to online privacy

<br> 

There are many different techniques and programs that you can use to keep your data private when browsing the web. However, you'll notice that the more private and anonymous you become, the less convenient it will be for you to browse without your data being leaked. Convenience has always been the opposition of security. The more secure you become, the less convenient it is to go about normal tasks. For instance, the most secure computer is one that is powered off, disconnected from any internet connection, and stored 100 feet underground, but that would make it *pretty* difficult to download files.  

In this guide, I'll cover some increasingly difficult steps that you can take in order to increase your browsing anonymity. Just as a reminder, it is **illegal** to hack, purchase, or exchange illicit information and using the following techniques will not prevent you from being caught. I am not yet a professional, just a cyber security enthusiast.

<br>

<br>

### **1. Hide your IP address. This can be done by either using a VPN or a Proxy.**

<br>

A proxy will hide your IP address from your ISP and any website that you're accessing but it won't encrypt any data nor will it hide your IP address from anyone who can intercept your traffic on its way to the proxy. Without the data being encrypted, your breadcrumbs can easily be traced back to your actual IP address. A proxy can be thought of as a middleman that makes sure requests get sent from a different IP address than the one you use. This might be enough for you if you are only trying to hide your IP address from your internet service provider (ISP), but I wouldn't recommend it. Proxies can log your browsing data which can lead to some sticky situations. Proxy servers can sell your data, attack your computer, or put you at risk of identity theft and other cybercrimes. Therefore it is typically more secure and anonymous to use a virtual private network (VPN).

A VPN is an encrypted connection from a device to a network. These will not only hide your IP from your ISP and websites, but VPNs also encrypt your data so it is useless in the hands of eavesdroppers. This will keep your data unknown from ISPs and websites that you access but it won't keep your data safe from the VPN provider themselves. 

This is why picking the correct VPN is *crucial*. VPNs can be owned by data conglomerates, nation-state actors, or even malicious threat actors (a.k.a. bad guys). Some VPNs can be forced to disclose information about you to a government depending on what country that data is physically located in. Some VPNs have even been known to collect data on their users for malicious reasons. Just because they are a VPN, doesn't mean they are any more trustworthy than your own ISP. There have been many large and "highly trusted" VPN services that aren't any more trustworthy than your ISP.
Knowing **who** your VPN providers parent company is, **what** their intentions are, and **where** they are located is extremely important for your own privacy. The best VPNs will have a "no logs" policy where your browsing data isn't logged. This prevents the use of that data for advertising, sales, or governments requesting your data.

There are many different trustworthy VPN services out there, but through my findings, I've come to use VyprVPN. It has a "no logs" policy, it is incorporated in Switzerland for privacy laws and internet rights, and their parent company has a healthy security culture.

> Use a VPN that has a "no logs" policy to hide your IP address and encrypt your traffic.

Even if your traffic isn't being logged by the VPN, they still have access to your unencrypted traffic. Even with the best VPN provider in the world, you still might not be able to trust them entirely. Where there is the potential for leaking information, it can and will happen. Because VPN providers can't be trusted, they won't be the only thing needed to remain private when browsing the internet. 

<br>

### **2. Use a browsing client that hides your IP address from your VPN. In other words, use Tor.**

<br>

Tor is a browser application, similar to Firefox but with additional privacy features. Tor is an open-sourced browser that will keep your VPN service from seeing what you're doing. Additionally, your VPN service will keep Tor from seeing who you are as your traffic is encrypted by the VPN. They are both used together to maximize security. 

It is *very important* to only use Tor with a VPN, as using it without a VPN can get you in trouble with your ISP. Also, Tor can be malicious in itself, so it is important to have encrypted traffic before using it.

Tor is typically much slower when it comes to browsing compared to other browsing clients. This is because it is sacrificing the convenience of a fast browser for that sweet, sweet anonymity. Tor uses a system of protocols that is similar to how a chain of proxies would act. This allows your traffic to be zig-zagged around the internet before it reaches your destination. Anyone trying to follow these traces would be the equivalent of going on a wild goose chase. This makes it *nearly* impossible to trace back your browsing data to you.

Tor keeps you anonymous by having at least 3 nodes or hops that your traffic goes through. 

* *An Entry Node*: This one knows your IP address and your traffic (inevitably)
* *The middle node, or Relay Node*: This one prevents any other nodes used from knowing where your traffic came from and who it came from.
* *The exit node*: this node knows what website you are accessing, but it doesn't know who you are.

This 3 node system separates your IP address from your internet requests and allows you to communicate to websites without any party knowing who the other is. Combine this with a VPN and your browsing traffic will be extremely difficult to trace back to you. However, this won't make it impossible to trace back to you, especially for the advanced cyber security professionals. This is because the exit node from Tor can be under control of a government agency or a malicious identity that will lead to you being identified. If someone really needs to find out who you are they can trace your hops through Tor, find out who your VPN provider is, and obtain your identity through legal action. Although this is still extremely unlikely and difficult, it can still happen. For most this step will be enough for people trying to browse the dark web but you can still be stealthier. 

In order to use Tor, you'll need to follow the process on the website, torproject.org. Downloading it involves installing another program called GnuPG that verify signatures. Verifying a signature of a program before it's installation ensures you aren't installing something that's been tampered with which is extremely important when downloading Tor.

So far, we've only mentioned your data being anonymous through your web browser, however there are many programs that don't use your web browser that still use the internet. One way to use multiple programs anonymously is to run them through an operating system called TAILS. This operating system can be installed on a single USB drive which brings us to our next step.

<br>

### **3. Use TAILS OS on a USB drive.**

<br>

TAILS stands for The Amnesic Incognito Live System and is aimed at preserving privacy and anonymity. It connects to the internet exclusively using Tor, but that's not the only feature that makes it more anonymous than other operating systems. TAILS always starts from the same clean state, it isolates applications from others, and it overwrites it's RAM when it's shutdown to prevent it from being recovered. It has a slew of privacy features to make sure you stay anonymous.

While you can also download TAILS OS to a separate computer or a virtual box, it's best to do it on a USB drive for a few reasons. One reason is that when you pull the USB drive out, all data on TAILS will be completely wiped leading no trace of what was done on your end. Not only will a separate computer be more expensive, but it will only be able to be used as an anonymous browser and nothing else for reasons I'll get to in the next step. Many hypervisors or virtual machines can contain information tracing them back to your device or they can be susceptible to attacks like hyperjacking, VM escape, and more.

This step is the next level of privacy. If you are doing the previous two steps on your everyday computer, you can still be identified if your machine is compromised. With using TAILS from a USB, an attacker wouldn't be able to access anything else on your everyday device. This is extremely useful as it keeps you anonymous in worse case scenarios. This will also obscure your hardware. When connecting to a website, your hardware and software may have a snapshot taken of it. This is information that can trace your browsing requests back to you. 

To download TAILS onto a USB, go to https://tails.boum.org/ to get started. TAILS is built for privacy and security, but even when using it, there are ways you can still be identified. This leads us to our final and most difficult step, don't be yourself.

<br>

### **4. Now that you are anonymous, you are a new person.**

<br>

The moment you log into your daily email account by mistake, you've leaked everything you've done back to you. If you're using TAILS from a USB you can unplug it to erase everything you did but your requests to those websites are still traceable back to you.

This is, without a doubt, the hardest part of being anonymous. It relies on your own discipline and diligence. Any accounts used previously can be traced back to you and your IP. This is why you will need to create new accounts for any website you intend on using. Failure to do so will render all previous steps completely useless. Additionally, you must not use any of these accounts outside of TAILS or Tor. If you log into your everyday-use password manager, you've already compromised your anonymity. If you want additional anonymity, it's best to create new accounts often. This way, your accounts can be used like a burner phone. Once you're done with the task at hand, delete the account. The more data that builds up on each account will make it easier to identify who owns that account.

Specific browsing habits and the way you write can be traced back to you. With the advent of machine learning, there's no telling what concoction of data can be used to find out who you really are. This is why it's important to not use TAILS or Tor like you would when you're normally browsing. Better yet, stay off of any social media altogether if possible. Don't use similar usernames or even passwords. The less actions you perform overall, the less chance there is of someone using that data to identify who you are. This is also the same reason that you don't want to be using your TAILS OS for very long either. 

Just as a reminder, it is **illegal** to hack, purchase, or exchange illicit information and even if you are able to do these 4 steps flawlessly, you can be found. With unlimited funds, any group determined enough *will* find out who you are. That's why it's best to just avoid these situations altogether by not doing anything illegal. If you're interested in figuring out how much jail time you can get for cybercrimes, then don't do anything illegal. Cybercrimes can be anywhere from 1 to 30 years or more in prison and $100,000 to $1,000,000 in fines.

**Have fun being anonymous!**

<br>

<br>

<br>

**Sources:**

https://www.whatismyip.com/are-proxies-safe/
https://www.expressvpn.com/vpn-service/tor-vpn
https://pentestlab.blog/2013/02/25/common-virtualization-vulnerabilities-and-how-to-mitigate-risks/
https://tails.boum.org/contribute/design/memory_erasure/
Book: The Art of Invisibility: The World's Most Famous Hacker Teaches You How to Be Safe in the Age of Big Brother and Big Data 

