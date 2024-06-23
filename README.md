# Setup Pi Hole within Docker on Raspberry Pi

This guide will walk you through the process of setting up Pi-hole, a network-wide ad blocker, within a Docker container on your Raspberry Pi.

## Prerequisites

Before you begin, make sure you have the following:

- Raspberry Pi 4 with Raspbian OS installed
- Docker installed on your Raspberry Pi 4
- Static IP assigned to your Pi in router.

## Raspberry Pi and Docker Setup:

### Raspberry Pi and Micro SD
To set up my Pi Hole I used a Raspberry Pi 4 with 32 GB Micro SD.

![Raspberian being written to SD Card](/public//images/raspberian-install.png)

### Docker and Docker Compose 🐳
I also installed Docker on my Raspberry Pi: I didn't want to re-install everything in case something goes wrong.

Once installed, I did make sure the user pi can use it (I don't want to use sudo every time)

```bash
curl -fsSl https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker pi
```

## Pi Hole Installation Steps

1.  Start by creating a directory where you will store the configuration file for the Pi-Hole docker container.
    ```bash
    sudo mkdir -p /opt/stacks/pihole
    ```
    and use `cd` command to switch to the newly created directory.

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

3. Use the following command to pull the image, create and run a docker container:

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
    If you don't know the IP address of your Raspberry Pi, just run

    ```bash 
    hostname -I

    192.168.1.162 172.17.0.1 172.29.0.1 169.254.70.85 172.21.0.1 169.254.52.242 2a00:23c7:8e8b:1201:f35:95a7:4c14:6ed
    ```

    So, in my case, my Raspberry's IP address is 192.168.1.162. It shouldn't change until you disconnect the Raspberry from the Internet > or you restart the router.

6. After the setup is complete, configure your router's DNS settings to use the IP address of your Raspberry Pi as the primary DNS server.

## Usage

You can now enjoy ad-free browsing on your network! Pi-hole will block ads and trackers at the network level.

To access the Pi-hole admin interface, open a web browser and navigate to `http://<your_raspberry_pi_ip_address>/admin`.
