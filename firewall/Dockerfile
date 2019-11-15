FROM ubuntu

RUN apt-get update && apt-get upgrade -y && apt-get install -y iputils-ping bridge-utils net-tools iptables

ENTRYPOINT sysctl -w net.ipv4.ip_forward=1 && \
           iptables -t nat -A PREROUTING -i eth0 -p tcp -m tcp --dport 80 -j DNAT --to-destination $WORDPRESS_IP && \
           # This monstrosity is used to get _our_ IP at the end.
           iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source $(ifconfig eth0 | grep 'inet' | cut -d: -f2 | awk '{print $2}') && \
           # Allow HTTP (TCP 80) only.
           # This blocks DNS, which might be an issue.
           # TODO: Currently disabled! This prevented forwarding from working, figure out a less restrictive configuration.
           # iptables -A FORWARD -p tcp --dport 80 -j ACCEPT && \
           # iptables -A FORWARD -j DROP && \
           /bin/bash
