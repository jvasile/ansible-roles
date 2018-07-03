This is an ansible role that uses certbot webroot to get Let's Encrypt
ssl keys.  It works on Debian boxes running Apache2.

You will need to set two variables:

     apache_server_name = the url you're making a cert for
     certbot_email = your email so you get reminders to renew

       vars:
          apache_server_name: 
            - sharks.withlasers.com
            - guppies.withlasers.com
          certbot_email = your@email.address.com

This role sets up a temporary apache configuration that handles
requests to the {{ apache_server_name }} domain.  It fulfills those
requests by looking in /var/www/certbot.

Certbot puts some secrets in that directory, then tells the Let's
Encrypt server to go to https://{{apache_server_name}} and try to get
them.  If that succeeds, then Let's Encrypt knows you have control
over this domain and it will issue you a set of ssl keys.

Because this role needs to point http://{{apache_server_name}} at the
certbot directory.  We don't run services on port 80 except to
redirect to 443.  If you already have a service on
https://{{apache_server_name}}, you'll have to comment out the port 80
redirection stuff in its config so as not to conflict with the certbot
port 80 config.

Note that you can add '--staging' to the certbot invocation below to
avoid hitting the Let's Encrypt rate limits.  Use that for testing if
you like.

This role uses the webroot method instead of just calling certbot
--apache.  We are, essentially, reinventing the --apache method.  The
certbot website says "Due to a security issue, Let's Encrypt has
stopped offering the mechanism that the Apache plugin previously used
to prove you control a domain." and then it gives a bunch of instrux
for getting around that.  This, to me, was a simpler way to do it via
ansible.  When certbot is fixed on Debian, maybe we should rewrite
this.

TODO: ansible galaxy
