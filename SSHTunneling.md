# Principles of tunneling for our needs #



Tunneling allows packets to pass from your machine A, through a server B to some other server C where:
  * B can be directly reached by A.
  * C is generally not directly reachable from A because B is a firewall, or because there is NAT-ing by B.
"Directly reachable" here meaning I can ping the machine that is like so for me.

B act as a tunnel forwarding packets coming from A and going to B, hence the name "tunneling".

In our case, we have done tunneling over an SSH connection.


# Ports to be tunneled #

  * SVN+SSH : 22
  * SIP (Session Initiation Protocol for Voice Over IP) : 5060 **(packets are UDP)**
  * HTTP : 80
  * MySQL : 3306

## Tunneling TCP-based protocols: easy ##
For all those protocols which are TCP-based except SIP, tunneling should be possible. We haven't been able to tunnel port 3306 for MySQL and decided to use [phpMyAdmin](http://www.phpmyadmin.net/) after that so as to edit our remote database over HTTP.

## Tunneling UDP-based protocols such as SIP ##
SIP will not tunnel naturally over SSH because SSH works in TCP and SIP in UDP.



# Going through tunnels as clients #

## Linux ##

You need ssh or can use custom tools such as our [multi\_tunneler's project's executables](http://code.google.com/p/multitunneler/wiki/downloads?tm=2).

If you use ssh do:

  * preventing man in the middle attack blockings (clear auth hosts file)

## Tunneling on Windows ##

**Requires:**
  * 