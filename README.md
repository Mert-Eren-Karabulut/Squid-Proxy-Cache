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

We have installed Squid, now we need to configure it to work in our LAN. As we installed Squid, configuration files are automatically generated. In order to edit that file use the comment below.

```
sudo nano /etc/squid/squid.conf
```

In nano editor we can use Ctrl+W to find lines that we need.

``` http_access allow localnet = remove the # symbol ```

Find ``` acl localnet ``` section and add the line below


``` acl localnet src YOUR IP RANGE ```

As example a typical home network uses ``` 192.168.1.0/24 ``` which means IP range from 192.168.1.0 to 192.168.1.255

Important Note: If there is another network preset which uses 192.168.x.x pattern, remove that line because it will cause conflicts otherwise.

Find
``` # dns_v4_first off remove the # symbol and change off to on. ```

Set
``` Cache_mem 256 MB ```

Set
``` Maximum_object_size 4096 MB ```

Set
``` Maximum_object_size_in_memory 8192 KB ```

As last step we will determine the cache directory


``` Cache_dir ufs /var/spool/squid3 = 8192 (1st variable - this is 8192 MB) ```


8192MB in that command is the capacity of the cache. After this amount of space is occupied by Squid, Squid will start deleting from the oldest cache. If you are an advanced user you can change the cache directory. 

In order to save press ``` Ctrl+X ``` and type ``` y ``` in order to exit Nano editor. 


We need to restart squid service to see changes.
``` sudo service squid restart ```

## Step 4 

Setup is completed, now we need to set our devices to use the proxy server that we created. In order to do so you must go to your devices proxy settings and set server ip to the ip of your squid proxy installed device. Default port is 3128 and can be changed in configuration file. Keep in mind that in order to use common ports like 80 or 8080 you need to give specific permissions to squid service.


## Testing if the system works

There are 2 ways of checking. You can download a file, delete it and download again to see if there is any speed difference.

Other way is checking service from squid access log. To do so type ``` sudo tail -f /var/log/squid/access.log ``` and look for changes when you click on a website from a client device. You can also see HIT and MISSes from that document.


I tested the system with 50MB test file and it was nearly instant when I tried to download it the second time.

Have fun.



