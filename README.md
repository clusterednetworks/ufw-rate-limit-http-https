# ufw-rate-limit-http-https
Rate limit traffic to your webserver with UFW
This is the UFW we add to our before.rules in UFW to prevent DDoS attacks on our webservers.
Works with Debian 9 / 10 and Ubuntu 18.04 & 20.04
# Usage
Add these lines to /etc/ufw/before.rules after<br>
<code># End required lines</code>
1. Add these lines<br>
<code># Start CUSTOM UFW added by clusterednetworks 2020-10-20<br>
<code># Limit to 20 concurrent connections on port 80/443 per IP<br>
<code>-A ufw-before-input -p tcp --syn --dport 80 -m connlimit --connlimit-above 20 -j DROP<br>
<code>-A ufw-before-input -p tcp --syn --dport 443 -m connlimit --connlimit-above 20 -j DROP<br>
<code># Limit to 20 connections on port 80/443 per 2 seconds per IP<br>
<code>-A ufw-before-input -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --set<br>
<code>-A ufw-before-input -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --update --seconds 2 --hitcount 20 -j DROP<br>
<code>-A ufw-before-input -p tcp --dport 443 -i eth0 -m state --state NEW -m recent --set<br>
<code>-A ufw-before-input -p tcp --dport 443 -i eth0 -m state --state NEW -m recent --update --seconds 2 --hitcount 20 -j DROP<br>
<code># End Custom UFW by clusterednetworks</code>

