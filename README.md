# SSL-enabled Postgres DB image + TimescaleDB and PostGIS

This repository contains the logic to build SSL-enabled Postgres images with the Supabase extensions already installed.

When you deploy the Supabase DB service from the Railway-published template, the image that is used is built from this repository.

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/postgres-supabase)

### Why though?

The [supabase/postgres](https://hub.docker.com/r/supabase/postgres) image in Docker hub does not come with SSL baked in.

Since this could pose a problem for applications or services attempting to connect, we decided to roll our own image with SSL enabled.

### How does it work?

The Dockerfiles contained in this repository start with the `supabase/postgres` image as base.  Then the `init-ssl.sh` script is copied into the `docker-entrypoint-initdb.d/` directory to be executed upon initialization.

#### Wrapper script

You will see in the Dockerfiles that the `ENTRYPOINT` is replaced with a `wrapper.sh` script.  The only logic the wrapper executes is to `unset` the `PGHOST` environment variable prior to executing the default entrypoint.  This logic is specific to Railway and allows us to use the `PGHOST` environment variable to enable the Database View.

If this logic conflicts with your use-case in any way, please feel free to spin up your own version of the image without the wrapper script.

#### Cert expiry
By default, the cert expiry is set to 820 days.  You can control this by configuring the `SSL_CERT_DAYS` environment variable as needed.

### A note about running the images locally

The images built from this repo and workflow do not contain support for ARM.  Check out the Dockerfiles that build them and feel free to copy the logic to build your own image with the appropriate base containing support for ARM.