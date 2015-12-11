# dokku-letsencrypt

dokku-letsencrypt is a plugin for [dokku][dokku] that gives the ability to automatically retrieve and install TLS certificates from [https://letsencrypt.org](letsencrypt.org). Contrary to other methods, no temporary disabling of the webserver is required during the ACME challenge procedure!

**Note:** `dokku-letsencrypt` will not autorenew the certificates (but you can run the included certificate renewal procedure in a cronjob).

## Installation

```sh
# dokku 0.4+
$ dokku plugin:install https://github.com/sseemayer/dokku-letsencrypt.git
```

## Commands

```
$ dokku help
    letsencrypt:on <app>                              Enable letsencrypt for app
```

## Usage

Obtain a Let's encrypt TLS certificate for app `myapp` (you can also run this command to renew the certificate):

```
$ dokku letsencrypt:on myapp
-----> Enabling letsencrypt for myapp...
-----> Updating letsencrypt docker image...
latest: Pulling from letsencrypt/letsencrypt
Digest: sha256:b7543399a2347b43c1d0f3b8c2a3deb8a9d3945fb762c0dbd1d595927813e9c4
Status: Image is up to date for quay.io/letsencrypt/letsencrypt:latest
       done
-----> Enabling ACME proxy for myapp...
-----> Getting letsencrypt certificate for myapp, domain myapp.mydomain.com...
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/myapp.mydomain.com/fullchain.pem.
   Your cert will expire on 2016-03-10. To obtain a new version of the
   certificate in the future, simply run Let's Encrypt again.
 - If you like Let's Encrypt, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

-----> Configuring SSL for myapp.mydomain.com...
-----> Creating https nginx.conf
-----> Running nginx-pre-reload
       Reloading nginx
-----> Disabling ACME proxy for myapp...
       done
```

## Design


`dokku-letsencrypt` will temporarily add a reverse proxy for the `/.well-known/` path of your app to handle ACME challenges, run [https://letsencrypt.readthedocs.org/en/latest/using.html#running-with-docker](letsencrypt as a docker container) with the 'standalone' authenticator and then remove the temporary reverse proxy from your app.


## License

This plugin is released under the MIT license. See the file [LICENSE](LICENSE).

[dokku]: https://github.com/progrium/dokku