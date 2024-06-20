# Setting up Pi-hole inside Docker on Raspberry Pi 4

This guide will walk you through the process of setting up Pi-hole, a network-wide ad blocker, inside a Docker container on your Raspberry Pi.

## Prerequisites

Before you begin, make sure you have the following:

- Raspberry Pi 4 with Raspbian OS installed
- Docker installed on your Raspberry Pi 4 (Can be installed with `curl -sSL https://get.docker.com | sh`)
- Static IP assigned to your Pi in router.

## Installation Steps

1.  Start by creating a directory where you will store the configuration file for the Pi-Hole docker container.
    ```bash
    sudo mkdir -p /opt/stacks/pihole
    ```
    and cd to newly created directory.

    ```bash
    cd /opt/stacks/pihole
    ```

2. Our next step is writing the “compose.yaml” file. This file is where we will define the Pi-Hole docker container and the options we want passed to the container. 

    ```bash
    sudo nano compose.yaml
    ```

    and add following contents to it and save the file:

    ```yaml
    version: "3"
    services:
        pihole:
            container_name: pihole
            image: pihole/pihole:latest
            ports:
                - "53:53/tcp"
                - "53:53/udp"
                - "67:67/udp"
                - "80:80/tcp"   # Use 8080:80/tcp if you have another service running on port 80
                - "443:443/tcp"
            dns:
                - 127.0.0.1
                - 1.1.1.1
            environment:
                TZ: 'Europe/London'     # https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
                WEBPASSWORD: "YOUR_PASSWORD_HERE"   # Replace this with your password. Pi-Hole will randomly generate the password if you don’t set a value.
            volumes:
                - './etc-pihole:/etc/pihole'
                - './etc-dnsmasq.d:/etc/dnsmasq.d'
            cap_add:
                - NET_ADMIN
            restart: unless-stopped

    ```

3. Pull the image, create and run a docker container:

    ```bash
    sudo docker compose up -d
    ```

4. Wait for the container to start. You can check the logs using the following command:

    ```bash
    docker logs -f pihole
    ```
    Or you can access the shell with:
    ```bash
    sudo docker exec -it pihole bash
    ```

5. Once the container is up and running, open a web browser and navigate to `http://<your_raspberry_pi_ip_address>`.

6. After the setup is complete, configure your router's DNS settings to use the IP address of your Raspberry Pi as the primary DNS server.

## Usage

You can now enjoy ad-free browsing on your network! Pi-hole will block ads and trackers at the network level.

To access the Pi-hole admin interface, open a web browser and navigate to `http://<your_raspberry_pi_ip_address>/admin`.
