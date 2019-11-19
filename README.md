# Firewall using Docker

A project that deploys a system with iptables using Docker.

Note: you might not need to `sudo` everything.

```bash
# Start by building the firewall and the pinger image.
cd firewall
sudo docker build -t firewall .
cd ../pinger
sudo docker build -t pinger .
cd ../httpget
sudo docker build -t httpget .
cd ..

```

```bash
# Then launch WordPress + MySQL.
# This terminal is going to output logs while the stack is running. Get another one.
cd wordpress
sudo docker-compose up
```

```bash
# Figure out the wordpress_wordpress_1 IP address.
sudo docker network inspect wordpress_frontend
# For me it was 172.25.0.2, so I will keep using this number.
# The following line runs the firewall and sets a few things:
#   Privilege in order to get sysctl to work (needed to enable IPv4 forwarding).
#   The network the frontend is in.
#   The network administrator capability.
#   The IPv4 WordPress has in this network.
sudo docker run --privileged --network=wordpress_frontend --cap-add=NET_ADMIN --env=WORDPRESS_IP=172.25.0.2 --interactive --tty firewall bash

# To test the connectivity use the container pinger

# The following line runs the pinger and set the addresses to ping:
#   The ADDRS variable must be a comma-separated list of the IPv4 addresses to ping

sudo docker run --env=ADDRS=172.25.0.2,172.25.0.3,172.26.0.2,172.26.0.3 --interactive --tty pinger bash 

# To run the pinger inside the wordpress_frontend network use:

sudo docker run --network=wordpress_frontend --env=ADDRS=172.25.0.2,172.25.0.3,172.26.0.2,172.26.0.3 --interactive --tty pinger bash 


# To test HTTP requests to the wordpress container use the container httpget
# The following line runs the container httpget inside the wordpress_frontend network 
# For me, it was able to get the page from the firewall and the wordpress container of the wordpress_frontend network, but fails when attempts with the other IPs

sudo docker run --network=wordpress_frontend --env=ADDRS=172.25.0.2,172.25.0.3,172.26.0.2,172.26.0.3 --interactive --tty httpget bash 


```

After all this is done, in my execution, the WordPress server was accessible in 172.25.0.2 (as one would expect) and in 172.25.0.3 (via forwarding).
