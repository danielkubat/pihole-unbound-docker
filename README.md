# Pi-hole & Unbound Docker setup for Raspberry Pi

This is a docker compose setup which starts a [Pi-hole](https://pi-hole.net/) and [nlnetlab's Unbound](https://nlnetlabs.nl/projects/unbound/about/) as upstream recursive DNS using official (or ready-to-use) images. The main idea here is to add security, [privacy](https://www.cloudflare.com/learning/dns/what-is-recursive-dns/) and have ad and malware protection, everything hosted locally.

If you want to learn more about why you want to have exactly this setup, read a [detailed explanation here](https://docs.pi-hole.net/guides/dns/unbound/).

## Understand the Setup

This setup works on a machine that does not itself already has DNS running (i.e. port 53 is already used). If you have a setup like that (e.g. running on a Synology NAS with a Directory Server), you would need a setup that creates a [Mac VLAN](https://docs.docker.com/network/macvlan/) so the container appears with a different IP. In this [case check out this example here](https://github.com/chriscrowe/docker-pihole-unbound/tree/main/two-container).

It is designed to have 2 containers running next to each other and do not aim to combine both programs in one. The idea is to minimize the work needed to adapt provided containerized versions of Pi-hole and Unbound, i.e. use the official images, therefore making it easier to upgrade each.

## Configuration

The main configuration can be set in the `.env` file which overwrites the ENV variables in the `docker-compose.yml` - change it to your liking:

```properties
WEBPASSWORD= # set the password to use in the Web Admin UI
HOST_IP_V4_ADDRESS= # the IP of the host the Pi-hole runs on - defaults to localhost
TIMEZONE= # set your timezone (used to schedule cron jobs e.g.)
```

## Deploy Containers

Start the stack with going to the root of the repo and do:

```bash
 docker compose up -d --remove-orphans
```

To stop everything:

```bash
 docker compose down
```

## Upgrade Base Images

In the `docker-compose.yml` change the used Pi-hole version by changing

```yaml
services:
  pihole:
    container_name: ...
    image: pihole/pihole:2024.05.0 # https://github.com/pi-hole/docker-pi-hole/releases
    hostname: ...
```

and Unbound 

```yaml
services:
  unbound:
    container_name: ...
    image: danielkubat/unbound-rpi:1.19.2 # https://github.com/danielkubat/unbound-rpi/releases
    hostname: ...
```

## License

Copyright 2023 Patrick Favre-Bulle
Copyright 2024 Daniel Kubat

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

```
https://www.apache.org/licenses/LICENSE-2.0
```

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
