# Guacamole with docker-compose
This is a small documentation how to run a fully working Apache Guacamole (incubating) instance with docker (docker-compose). The goal of this project is to make it easy to test Guacamole.

## About Guacamole
Apache Guacamole (incubating) is a clientless remote desktop gateway. It supports standard protocols like VNC, RDP, and SSH. It is called clientless because no plugins or client software are required. Thanks to HTML5, once Guacamole is installed on a server, all you need to access your desktops is a web browser.

It supports RDP, SSH, Telnet and VNC and is the fastest HTML5 gateway I know. Checkout the projects homepage for more information.

## Prerequisites
You need a working docker installation and docker-compose running on your machine. More info about Docker and how to install it on Debian/Ubuntu:

        https://github.com/rozicdejan/Docker-help.git
## Quick start
Clone the GIT repository and start guacamole:

    git clone "https://github.com/boschkundendienst/guacamole-docker-compose.git"
    cd guacamole-docker-compose
    ./prepare.sh
    docker-compose up -d
Your guacamole server should now be available at https://ip of your server:8443/. The default username is guacadmin with password guacadmin.

Details
To understand some details let's take a closer look at parts of the docker-compose.yml file:

## Networking
The following part of docker-compose.yml will create a network with name guacnetwork_compose in mode bridged.

...

          # networks
          # create a network 'guacnetwork_compose' in mode 'bridged'
          networks:
            guacnetwork_compose:
              driver: bridge
...

## Services
guacd
The following part of docker-compose.yml will create the guacd service. guacd is the heart of Guacamole which dynamically loads support for remote desktop protocols (called "client plugins") and connects them to remote desktops based on instructions received from the web application. The container will be called guacd_compose based on the docker image guacamole/guacd connected to our previously created network guacnetwork_compose. Additionally we map the 2 local folders ./drive and ./record into the container. We can use them later to map user drives and store recordings of sessions.

      services:
        # guacd
        guacd:
          container_name: guacd_compose
          image: guacamole/guacd
          networks:
            guacnetwork_compose:
          restart: always
          volumes:
          - ./drive:/drive:rw
          - ./record:/record:rw
...
