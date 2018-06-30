---
author: Sarath
title: "HowTo: Installing LetsEncrypt SSL certificate on AWS Bitnami EC2"
date: 2017-02-05T15:12:42+05:30
categories:
  - Web
tags:
  - Letsencrypt
  - AWS
  - Wordpress
draft: false
---

This blog is hosted at AWS on an EC2 VPS which is a Bitnami WordPress image. I decided to switch the blog to serve over HTTPS and in turn try out [Let’sEncrypt](https://letsencrypt.org/) which is a free, automated Certificate Authority(CA).

With Let’Encrypt, certificate issuance is an automated process hence it is required to use an [ACME](https://ietf-wg-acme.github.io/acme/) client to prove that we have control over the domain for which we want to register the certificate for. I have used the recommended [Certbot](https://certbot.eff.org/) software.

### The important steps are:

1. Install the ACME software on the web host
2. Run the ACME client and generate the SSL certificate for the domain
3. Update Apache to use the new SSL certificates and redirect all HTTP traffic to HTTPS

Detailed steps are as follows

1. ssh into the EC2 instance
2. Create dir for Certbot
<pre><code>$ mkdir certbotsetup
$ cd certbotsetup
</pre></code>
3. Install Certbot  
<pre><code>$ wget https://dl.eff.org/certbot-auto
$ chmod a+x certbot-auto
</pre></code>		   

4. To obtain the certificate for the domain, say sarathshyam.in, run Certbot with the following command. The path after -w is the wordpress root directory. Enter a valid email id when prompted (Let’sEncrypt will email to this id for notifications regarding certification expiration).
For more details, [refer](https://certbot.eff.org/lets-encrypt/ubuntutrusty-other)
<pre><code>$ ./certbot-auto certonly --webroot -w /home/bitnami/apps/wordpress/htdocs/ -d sarathshyam.in -d www.sarathshyam.in</pre></code>

5. Once the certificates are successfully acquired, edit the apache config to use the certificate.
<pre><code>$ sudo nano /opt/bitnami/apache2/conf/bitnami/bitnami.conf</pre></code>
Comment out the below lines
<pre><code>#SSLCertificateFile "/opt/bitnami/apache2/conf/server.crt"
#SSLCertificateKeyFile "/opt/bitnami/apache2/conf/server.key"
</pre></code>		   
And add the path to the new certificates
<pre><code>SSLCertificateFile "/etc/letsencrypt/live/sarathshyam.in/fullchain.pem"
SSLCertificateKeyFile "/etc/letsencrypt/live/sarathshyam.in/privkey.pem"
SSLCACertificateFile "/etc/letsencrypt/live/sarathshyam.in/fullchain.pem"
</pre></code>

6. Reroute all http traffic to https. [Refer](https://docs.bitnami.com/aws/components/apache/#how-to-force-https-redirection-for-an-application) for details.
<pre><code>$ nano /home/bitnami/apps/wordpress/conf/httpd-prefix.conf</pre></code>
Add the following lines to the top
<pre><code>RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^/(.*) https://%{SERVER_NAME}/$1 [R,L]
</pre></code>

7. Done!. Verify whether the correct certificate is shown in the browser.

![SSLed..](/img/letsencrypt.png)


