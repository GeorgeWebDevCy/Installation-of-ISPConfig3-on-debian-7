# Installation of ISPConfig3 on debian 7

These are instructions for installing ISPConfig3 on debian 7
1. Make sure you have an A Record setup for your server
2. Install debian 7 using the minimal iso about 200MB
3. When asked for a hostname enter a name for your machine e.g. machine1
4. When asked for a domain name enter you domain name example.com this domain must be real and owned by you
5. So your fully qualified domain name will be machine1.example.com which is what you set as your A Record at your registrar
6. When tasksel comes up asking what to install just leave Basic system utilities anything else make sure you deselect.
7. When logged in to your server as root set you static ip as follows:
issue the command nano /etc/network/interfaces and make sure you set your static IP
auto eth0
iface eth0 inet static
    address xxx.xxx.xxx.xxx
    netmask xxx.xxx.xxx.xxx
    gateway xxx.xxx.xxx.xxx
8. restart your server and login as root again
9. Issue the command ping google.com if you get error you made a mistake in /etc/network/interfaces
10. also make sure you add you external ip to /etc/hosts as follows
External IP         Fully Qualified Domain Name       Hostname
xxx.xxx.xxx.xxx     machine1.example.com              machine1

11. Assuming you what ssh access to your server do this apt-get install openssh-server
12. apt-get update && apt-get upgrade
13. apt-get install unzip
14. cd /tmp
15. wget --no-check-certificate https://github.com/servisys/ispconfig_setup/archive/master.zip
16. unzip master.zip
17. cd ispconfig_setup-master
18. During installation we selected all defaults except that for XCache we said No since we need ioncube loader for softaculous later
19. We selected Roundcure as our webmail interfaces
20. Configure phpmyadmin with db-config? must be No
21. Answered no to backports
22.  nano /var/lib/roundcube/plugins/ispconfig3_account/config/config.inc.php
and and the username and password of the REmote user you created for ISPConfig. Don't change the url in the file

23. Download the latest ioncube loaders with wget and unpack the archive:
cd /tmp
wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
tar xfz ioncube_loaders_lin_x86-64.tar.gz

Move the loaders to /usr/local/ and clean up the /tmp directory
mv ioncube /usr/local/
rm ioncube_loaders_lin_x86-64.tar.gz

Configure PHP

Now edit the php.ini files with a editor like vi or nano:
For mod_php:
vi /etc/php5/apache2/php.ini

For CGI and FCGI PHP:
vi /etc/php5/cgi/php.ini

For PHP commandline scripts:
vi /etc/php5/cli/php.ini

For scripts running with PHP-FPM
vi /etc/php5/fpm/php.ini

and add the following line right at the beginning of the file(s) (before the [PHP] line):
zend_extension = /usr/local/ioncube/ioncube_loader_lin_5.4.so
so the the resulting file looks like this:
zend_extension = /usr/local/ioncube/ioncube_loader_lin_5.4.so
[PHP]

;;;;;;;;;;;;;;;;;;;
; About php.ini   ;
;;;;;;;;;;;;;;;;;;;
; PHP's initialization file, generally called php.ini, is responsible for
; configuring many of the aspects of PHP's behavior.
[...]
Finally restart apache to apply the changes:
service apache2 restart

When you use PHP-FPM, then restart the PHP-FPM pool daemon as well:
service php5-fpm restart
.............................................to be ocntinued
