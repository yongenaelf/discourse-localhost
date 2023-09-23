# Guide on how to have a local instance of Discourse to play around with

Install the devcontainer extension for VS Code and open this workspace in the devcontainer.

## Setup

```bash
git clone https://github.com/discourse/discourse_docker.git /var/discourse
cd /var/discourse
chmod 700 containers
./discourse-setup
```

Use the following as responses (press enter if empty):

```bash
Hostname for your Discourse? [discourse.example.com]: localhost:8080
In order to check the connection to localhost:8080:443 we need to open a socket using netcat.
However netcat is not installed on your system. You can continue without this check
or abort the setup, install netcat and try again.
Would you like to continue without this check? [yn] y
Skipping port check.
Email address for admin account(s)? [me@example.com,you@example.com]: admin@localhost
SMTP server address? [smtp.example.com]: smtp.localhost
SMTP port? [587]:
SMTP user name? [user@example.com]: smtp@localhost
SMTP password? [pa$$word]: localhost
notification email address? [noreply@localhost:8080]:
Optional email address for Let's Encrypt warnings? (ENTER to skip) [me@example.com]:
Optional Maxmind License key (ENTER to continue without MAXMIND GeoLite2 geolocation database) [1234567890123456]:
```

Setup will take some time to complete.

## Disable SSL

To access on localhost without SSL we can disable it.

```bash
cd /var/discourse/containers
nano app.yml
```

Comment out the following:

```yml
templates:
  #- "templates/web.ssl.template.yml"
  #- "templates/web.letsencrypt.ssl.template.yml"
```

Ctrl+O, press enter, then Ctrl+X.

Rebuild discourse:

```bash
cd /var/discourse
./launcher rebuild app
```

## Add an admin account

Since we skipped setting up the mail service, we need to manually create the admin account.

```bash
cd /var/discourse
./launcher enter app
rake admin:create
```

Follow the prompts to create an admin account.

## Cleanup

Once you are done, you can delete the containers, images and volumes using Docker Desktop to reclaim your space.

## References

- [Official installation docs](https://github.com/discourse/discourse/blob/main/docs/INSTALL-cloud.md)
- [Manually create the admin account](https://meta.discourse.org/t/troubleshoot-email-on-a-new-discourse-install/16326#need-to-log-in-without-receiving-a-registration-email-11)
- [Install plugins](https://meta.discourse.org/t/install-plugins-in-discourse/19157)
- [Private Topics Plugin](https://meta.discourse.org/t/private-topics-plugin/268646)
