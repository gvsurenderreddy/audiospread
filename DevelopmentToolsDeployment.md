# Deployment of tools for development #

## Our hardware and software setup ##
For information, Interspread uses the following software tools to develop the AudioSpread solution:
  * On our custom server machine:
    * An [SVN (or Subversion) server](http://subversion.tigris.org)
    * An [LAMP stack](http://fr.wikipedia.org/wiki/LAMP): a [Debian-based](http://www.debian.org) [Ubuntu Linux](http://www.ubuntu.org) distribution running Apache, MySQL and PHP,
    * [Drupal CMS,](http://www.drupal.org)
    * [Asterisk.](http://www.asterisk.org)

  * On each developer and customer machine:
    * An [SVN client](http://en.wikipedia.org/wiki/Comparison_of_Subversion_clients) (_for developers only_),
    * A SIP or H.323 [Voice-over-IP Softphone client,](http://en.wikipedia.org/wiki/Comparison_of_VoIP_software#General_softphone_clients)
    * A Web Browser,
    * An [SSH client](http://en.wikipedia.org/wiki/Comparison_of_SSH_clients).

The SSH tunneling constraint comes from our server location and is not a requirement to get the same functionality as ours. Apart from that, having SSH access to the server may help for remote maintenance after initial setup.
The final end-user solution should see the communications ported from VoIP to regular phone technology. This however is out of the project scope for now.

This diagram below shows quickly how we plan to make a web app out of an SVN repository. Please refer to the info on the picture itself for credits (it was not done by us).
![http://blog.creaone.fr/public/svn.png](http://blog.creaone.fr/public/svn.png)


Below is a diagram of our deployment: we have posted a [Dia source file](http://audiospread.googlecode.com/files/Tools_Deployment_For_Development.dia) of it in the [Downloads section](http://code.google.com/p/audiospread/downloads/list).

![http://audiospread.googlecode.com/files/DevelopmentWorkDeployment.png](http://audiospread.googlecode.com/files/DevelopmentWorkDeployment.png)

# Random links & notes #
http://subversible.com/ <= svn sync of Drupal's >= 5.x core and most used modules. Would spare from having to checkout modules from cvs.