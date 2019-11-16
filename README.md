# Firewall using Docker

A project that deploys a system with iptables using Docker.

Note: you might not need to `sudo` everything.

```bash
# Start by building the firewall and the pinger image.
cd firewall
sudo docker build -t firewall .
cd ../pinger
sudo docker build -t pinger .
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

# The following line runs the pinger and set the addresses to ping:
#   The ADDRS variable must be a comma-separated list of the IPv4 addresses to ping
#   The line bellow pings both the database and the wordpress services and also pings the firewall
sudo docker run --env=ADDR=172.25.0.1,172.25.0.2,172.25.0.3 --interactive --tty pinger bash 

# For me, when the pinger runs outside the wordpress_frontend network it fails to reach both wordpress and firewall but is able to reach the database (which seems wrong)
# To run the pinger inside the wordpress_frontend network use:
sudo docker run --network=wordpress_frontend --env=ADDR=172.25.0.1,172.25.0.2,172.25.0.3 --interactive --tty pinger bash 

# When executed inside this network the pinger is able to reach all the three tested addresses

```

After all this is done, in my execution, the WordPress server was accessible in 172.25.0.2 (as one would expect) and in 172.25.0.3 (via forwarding).
