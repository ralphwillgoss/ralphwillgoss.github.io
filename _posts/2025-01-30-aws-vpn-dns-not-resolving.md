---
title: "Why is my AWS VPN not resolving domain names via DNS?"
date: 2025-01-30
categories:
  - aws vpn dns
tags:
  - aws vpn dns
---

After upgrading the AWS VPN Client to version 5.0, I noticed that my domain names mapped to internal VPC IP's could no longer be resolved.

# Context
- I'm connected to an AWS VPC via the [AWS VPN Client](https://aws.amazon.com/vpn/client-vpn-download/)
- When attempting to resolve a DNS record which points to a service running in the VPC, this fails to resolve  
  _e.g._ `api.digitalsuperglue.com.cloud -> 10.1.2.3`
- I can successfully connect to the service using its VPC IP, therefore I know the VPN is and IP routing are working

_Note: For this AWS VPN configuration, it does NOT have preset primary and secondary DNS servers._

# Solution
I discovered that sometimes people run into DNS issues with the AWS VPN Client, when running a local network which is similar to what your AWS VPC is using. I've noticed a quick search can reveal [others who have reported a similar issue](https://stackoverflow.com/a/75877065/743).

One way to fix this is to add the Google DNS servers, to your AWS VPN Interface.  
For Windows, I did the following:

1. Go to `Control Panel -> Network and Internet -> Network Conenctions` and find the AWS VPN Interface:

   ![AWS VPN Network Interface](/assets/images/posts/aws-vpn-dns/aws-vpn-adapter-properties.png)  
<br/>
2. On the **Networking** tab, select the `Internet Protocol Version 4 (TCP/IPv4)` option and click on **Properties**:

   ![Network Interface DNS Settings](/assets/images/posts/aws-vpn-dns/adapter-1-networking-ipv4-properties.png)
<br/>

3. On the **General**  tab, click `Use the following DNS server addresses` radio button and enter **8.8.8.8** and **4.4.4.4** for the two DNS inputs:

   ![Network Interface DNS Settings](/assets/images/posts/aws-vpn-dns/adapter-2-ipv4-properties-dns.png)

   Adding the DNS servers manually worked for me, now when using the VPN Client it resolves the domain names as expected.