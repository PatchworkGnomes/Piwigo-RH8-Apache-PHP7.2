# Piwigo-RHEL8-Apache-PHP7.2

## Getting Started

Had someone ask me to create a photo gallery, so they could keep track of paints and glazes information as an artist.  Eventually I built something, but I thought there had to be something out there already. Stumbled across Piwigo, so just made a small "how to" for it, of course, more for me then anyone else.
<br />
<br />
I could always use the help. So, please…without hesitation, give me feedback.  More important, give me the best practice. I have no qualms with people showing and telling me how to better my limited knowledge.
<br />
<br />
Quote by Ram Dass: (or << Backtrack)<br /> 
“The quieter you become, the more you can hear.”

## Dependencies
<p style="color:red;"><b>Using Red Hat DISA-STIG security profile</b></p>
Keep in mind of Internal and External DNS and rules on your routers\firewalls, assuming you want this to be public.
Also note that the versions may be different, based on when you install
<ul>
    <li>Piwigo 13.8.0</li>
    <li>Red Hat Enterprise Linux 8.8 (Ootpa)</li>
    <li>10.3.35-MariaDB MariaDB Server</li>
    <li>Apache/2.4.37 (Red Hat Enterprise Linux)</li>
    <li>PHP 7.2.24</li>
    <li>certbot 1.22.0</li>
    <li>Python3</li>
    <li>Fail2Ban v1.0.2</li>
</ul>

## Installing\Executing 

What I like to do for this is log in as root or sudo su -.  then copy the script into the /tmp/ and then kick it off.
The first part of the script is for variables.</br> 
A few things to consider first:

### SERVER STUFF
This is basically going to be your external DNS name for your website.
Its what you will enter into the URL
</br>
<b>SERVERNAME=Piwigo.Your.Domain</b>

### CERT STUFF
The last line on the Script enables CertBot to create the SSL cert for the site. It is commented out, so make sure you have the External DNS setup properly and your firewall rules setup.  CertBot also needs port 80 and 443.  Please be careful how you setup your firewalls\NAT\Port forwarding

I have 2 ways of doing Certs (HTML-01 and DNS-01) in this script I lay the ground work for HTTP-01.
<ul>
    <li>HTTP-01 challenge: This challenge asks you to prove that you control the HTTP service for the domain in question. You do this by placing a specific file with a specific name and content in the .well-known/acme-challenge/ directory of your website.</li>
    <li>DNS-01 challenge: This challenge asks you to prove that you control the DNS for your domain. You do this by placing a specific DNS TXT record in your domain's DNS settings.</li>
</ul>
In general, most people use the HTTP-01 challenge because it's straightforward and can be easily automated with the Certbot tool, provided that port 80 is open to the internet. The DNS-01 challenge is the only method that can be used to issue wildcard certificates (*.yourdomain.com).
</br>

Email for Certbot that will notify when the Cert is expiring. Doesn't have to be real, but recommend
</br>
<b>CERTMAIL=your @ email.com</b>
 
### DB STUFF
This is the name of the database that will create for nextcloud
</br>
<b>DBNAME=piwigodb</b>

This is the username that is needed for the MediaWiki database.
</br>
<b>USER=piwigouser</b>

Email for Piwigo Admin account</br>
Doesn't have to be real, but recommend.  Also consider there is no postfix or email sending protocol
</br>
<b>APPMAIL=your @ email.com</b>

### PASSWORD STUFF
This generates a random password for the root account of mariadb. You can change it if you wish
</br>
<b>date +%s | sha256sum | base64 | head -c 16 > /tmp/.MARIADBPASSWORD</b>

This generates a random password for the MediaWiki user account in mariadb. You can change it if you wish
</br>
<b>date +%s | sha256sum | base64 | head -c 16 >> /tmp/.APPPASSWORD</b>

### EXTRA


### CLEANING UP
When the script is done running it will remind you that there are passwords and scripts in the /tmp. Keep them there as long as you need them, but do remember to clean house.  If a bad actor popped into you machine, they would have the key to your kingdom.

## Final Installation
When the script is done running, you will see in the terminal the information to finish the installation.</br>
When you goto <b>http://ip</b> you should see something like this:</br>
<p align="center">
  <img src="images/Mediawiki(step-01).png" >
</p>

</br>
Just go ahead and click the link <b>Complete the installation</b> and follow the instructions
</br>
Eventually you will get to the part when it asks you for Database info, this info will be in the terminal and\or /tmp/ directory
</br>
<p align="center">
  <img src="images/Mediawiki(step-03a).png" width="264" >
</p>
</br>
</br>
Continue with the process and eventually you will get here:
</br>
</br>
<p align="center">
  <img src="images/Mediawiki(step-08a).png" width="294" >
</p>
</br>
</br>
Now you have to download the <b>"LocalSettings.php"</b> and upload it to <b>/var/www/html/mediawiki</b>
</br>
Then click <b>enter your wiki</b>
</br>
That should be all.
