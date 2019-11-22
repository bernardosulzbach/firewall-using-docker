# Firewall using Docker

A project that deploys a system with iptables using Docker.

Note: you might not need to `sudo` everything.

For this project, static IPs were used to simplify the examples.

+ 172.30.0.2 (MySQL)
+ 172.30.0.3 (WordPress)
+ 172.30.0.4, 172.31.0.2 (Firewall)

```bash
# Build the images.
bash build-images.sh
```

```bash
# Create the required networks.
bash create-networks.sh
```

```bash
# Then launch WordPress + MySQL.
cd wordpress
sudo docker-compose up
# This terminal is going to output logs while the stack is running. Get another one.
```

```bash
# Run the firewall container.
sudo docker run --privileged --network=wordpress_frontend --cap-add=NET_ADMIN --env=WORDPRESS_IP=172.28.0.2 --interactive --tty firewall bash
# This will now show the firewall output, get another terminal.
```

```bash
# Add the firewall to its own network, our public one.
sudo docker network connect firewall firewall
# To test HTTP requests to the WordPress container use the container httpget.
sudo docker run --network=firewall --env=ADDRS=172.30.0.2,172.30.0.3,172.30.0.4,172.31.0.2 --interactive --tty httpget bash
```

```
> Failed to retrieve page from 172.30.0.2
> Failed to retrieve page from 172.30.0.3
> Failed to retrieve page from 172.30.0.4
> Sucessfully retrieved page from 172.31.0.2
```
