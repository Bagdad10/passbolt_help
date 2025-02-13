---
title: Upgrade from CE to Pro using docker
card_title: From CE with Docker
card_teaser: Upgrade from CE to Pro using docker
card_position: 6
date: 2018-04-09 00:00:00 Z
description: Upgrade from CE to Passbolt Pro using docker
icon: fa-docker
categories: [hosting,upgrade,pro]
sidebar: hosting
layout: default
slug: upgrade-pro-from-ce-docker
docker_tag: '-pro'
passbolt_version: Pro
permalink: /:categories/:slug.html
---

{% include layout/row_start.html %}
{% include layout/col_start.html column="7" %}

This tutorial covers the case where you want to upgrade from your passbolt CE into Passbolt Pro when using docker.

{% include messages/warning.html
    content="**Important:** Please take a full [backup](/hosting/backup) of your passbolt before proceeding with the upgrade. Backup should include passbolt files as well as the database."
%}

{% include messages/warning.html
    content="**Deprecation notice:** From version 3 of Passbolt config volume should be mounted on `/etc/passbolt` and sources one in `/usr/share/php/passbolt`."
%}


In order to upgrade from CE to Pro users just need to download the pro container and mount their corresponding subscription key.

{% include hosting/docker/docker-getting-containers.md %}

### Using host bind mounts

Users that use host bind mounts from host machine into docker file must adjust paths of the mounted files:

In the following snippet:
- passbolt_images_dir: path to a host directory that contains passbolt images Avatar directory.
- gpg_host_dir: path to a host directory that contains serverkey.asc and serverkey_private.asc

```bash
$ docker run --name passbolt{{ page.docker_tag }} --net passbolt_network \
             --mount type=bind, \
               source=<passbolt_images_dir>,\
               target=/usr/share/php/passbolt/webroot/img \
             {%- if page.passbolt_version == 'Pro' %}
             --mount type=bind,\
               source=<path_subscription>,\
               target=/etc/passbolt/license \
             {% else %}
             {% endif -%}
             --mount type=bind, \
               source=<gpg_host_dir>, \
               target=/etc/passbolt/gpg \
             -p 443:443 \
             -p 80:80 \
             -e DATASOURCES_DEFAULT_HOST=mariadb \
             -e DATASOURCES_DEFAULT_PASSWORD=<mariadb_password> \
             -e DATASOURCES_DEFAULT_USERNAME=<mariadb_user> \
             -e DATASOURCES_DEFAULT_DATABASE=<mariadb_database> \
             -e APP_FULL_BASE_URL=https://mydomain.com \
             passbolt/passbolt:latest{{ page.docker_tag }}
```

### Using docker volumes

Users that use docker volumes should adjust their volumes paths.

```bash
$ docker run --name passbolt{{ page.docker_tag }} --net passbolt_network \
             --mount source=<passbolt_images_volume>,\
               target=/usr/share/php/passbolt/webroot/img \
             {%- if page.passbolt_version == 'Pro' %}
             --mount type=bind,\
               source=<path_subscription>,\
               target=/etc/passbolt/license \
             {% else %}
             {% endif -%}
             --mount source=<gpg_keys_volume>, \
               target=/etc/passbolt/gpg \
             -p 443:443 \
             -p 80:80 \
             -e DATASOURCES_DEFAULT_HOST=mariadb \
             -e DATASOURCES_DEFAULT_PASSWORD=<mariadb_password> \
             -e DATASOURCES_DEFAULT_USERNAME=<mariadb_user> \
             -e DATASOURCES_DEFAULT_DATABASE=<mariadb_database> \
             -e APP_FULL_BASE_URL=https://mydomain.com \
             passbolt/passbolt:latest{{ page.docker_tag }}
```
