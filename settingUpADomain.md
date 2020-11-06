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

SSL (Secure Sockets Layer) and TLS (Transport Security Layer) are protocols for creating authenticated and encrypted links between machines on the internet. It works by associating the identity of a website to a crypto keypair(private and public keypair). This public key is available via the SSL certificate.

The certificate itself is signed by a **CA (Certificate Authority)** and is then deemed to be trusted by client side softwares like browsers etc.

### HTTPS

A website using the HTTPS protocol owns an SSL/TLS certificate that is signed by a valid CA. This certificate is available in different validation levels. An HTTPS connection ensures the following:
1. The server providing the site is in possession of the private keys that is paired to the public key in the certificate
2. The data flowing in from the server/website has not been tampered with by a man in the middle
3. The to and fro communication is encrypted. No data is sent in plaintext over the network

### Certificate Signing Request (CSR)

A CSR is generated on the server that needs to be secured using various methods depending on the platform. The way i do it is by using `openssl` which works nicely and is readily available on all `*nix` systems.

This CSR can be provided to any CA after you buy a certificate. After providing the CSR one needs to `Authenticate` their identity as the owner of the domain in question. Before that we need to setup our domain to be run by a server. This can be done by various methods and is dependent on the DevOps stack being used but here we consider a simple Linux VM with `nginx` installed and running.

### Setting up A VM

This section has a prerequisite. We need to have a Linux machine accessible via SSH. This VM should have a dedicated IP address and network settings where we can setup DNS Zones and add different record sets.

If the VM has a public IP address it must have a nameserver attached to it. This information is available in the `NS` record set.

Next step is using `apt` to install `nginx` then just do 

```bash
sudo service nginx start
```

Now when you visit your public IP from a browser you should see the nginx default homepage

Our VM is now ready. We can further configure `nginx` to act as a reverse proxy for a static frontend or an HTTP Server running on a specific port. But that's unrelated to the following steps.

### Setting up Record Sets

Record Sets are a part of the DNS system and contain useful information regarding websites. Some common record sets are:

1. `A`/`AAAA`: This is a basic record set that's used to point a domain name to an `IPv4` address. `AAAA` is similar just that it uses `IPv6`
2. `CNAME`: Used to link a subdomain to the domains `A` recordset. For example `example.com` and `www.example.com` can point to the same IP address by using a CNAME record for the `www` subdomain