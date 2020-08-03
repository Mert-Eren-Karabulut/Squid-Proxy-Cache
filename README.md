# What is proxy caching and why people need it?

Proxy caching is often used in LANs with low speed internet access. Main principle of proxy caching is storing web pages, downloaded files etc. in a 
server on lan in order serve this content directly from LAN while requested again with high speed while not using any internet bandwidth.


Everything that users request must pass from proxy in order to
go to internet. When Squid is the proxy server of a network, it works with a hit and miss methodology in order to pick connections that are able to be cached.


Majority of HTTP traffic can be cached alongside with some UDP traffic. Caching HTTPS is possible but requires much more compute power mainly because of the encryption and decryption
proccesses. So this guide is for caching HTTP traffic.


# Installation Steps


## Prerequests

A recent linux distro like Ubuntu 20.x or Windows WSL

Computer with at least 4Gb RAM, decent CPU and 20Gb free space.


## Step 1

In order to use cache server without any problems we must take a static IP. There are so many tutorials about how to get a static IP for both
Linux and Windows. 


Keep in mind that Windows WSL uses same IP with windows, so if you have anything that can block Ubuntu WSL's internet connection like antivirus,
don't forgot to disable them or set proper exceptions for Ubuntu WSL.


## Step 2
Ensure that your system is up-to-date and install required packages.

This commands will check if there is any possible update for already installed packages. We are doing this in order to prevent any conflict
while setting up Squid.


```
sudo apt update 
sudo apt upgrade
```


Now we can install Squid.


```
sudo apt-get install squid
```

## Step 3




