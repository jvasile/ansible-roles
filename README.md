# ansible-roles

These are some ansible roles I'm using.  I hope they are useful to you
too.  Please do hit me with improvements, especially documentation
that makes it easier for newbs.

# How To Use

I am not an ansible expert.  There are much better and more
sophisticated ways to use it.  The below is quick and self-contained,
though, which is why I like it.

I put something like this in a `foo.example.com.yml` file:

    ---
    - hosts: foo.example.com
      roles:
        - { role: apache-certbot, tags: ["apache-certbot"] }
    
      vars:
        apache_server_name:
           - foo.example.com
           - bar.example.com
        certbot_email: reminders@mydomain.com


And don't forget your `hosts` file:

    all:
      hosts:
        foo.example.com:
          ansible_user: root


And then I can run it from that dir with:

    ansible-playbook -i hosts foo.example.com --tags apache-certbot
