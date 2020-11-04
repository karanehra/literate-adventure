# Setting Up a Domain With a VM

A domain setup requires the following basic step:

1. Buying and Setting up a Domain on a provider
2. Buying an SSL certificate
3. Buying a VM instance to connect the Domain to
4. Setting up the server


## Setting Up a Domain

This step is straigtforward. Just go to a Domain Buying website like GoDaddy etc and buy a domain. Every domain has something called `nameservers` assigned to it.

### What is a Nameserver

A nameserver is like a phonebook. To access a website `xyz.com` while requesting our browser sends a request to the nearest nameserver(phonebook) to tell it the `IP Address (Phone Number)` of `xyz.com` so that we can reach it.

The whole process looks like the following: 

1. We tell our browser we want to visit `xyz.com`
2. Our computer then uses `DNS` to retrieve the current `nameservers` for `xyz.com` which come out to be `ns1.xyz.com` or `ns2.xyz.com`.
3. The computer asks for the `A-Record Set` which cotains the IP Address of `xyz.com` for example `192.15.237.216` which is where the contents of `xyz.com` reside
4. The browser then sends a request to this IP address and fetches the content for us
  
So in light of the above we will need to change our nameservers to the appropriate providers while setting it up.


## Buying an SSL Certificate

### What is SSL/TLS

SSL (Secure Sockets Layer) and TLS (Transport Security Layer) are protocols for creating authenticated and encrypted links between machines on the internet. 