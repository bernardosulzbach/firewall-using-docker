# Firewall using Docker

A project that deploys a system with iptables using Docker.

Note: you might not need to `sudo` everything.

```bash
# Start by building the firewall image.
cd firewall
sudo docker build -t firewall .
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
```

After all this is done, in my execution, the WordPress server was accessible in 172.25.0.2 (as one would expect) and in 172.25.0.3 (via forwarding).
