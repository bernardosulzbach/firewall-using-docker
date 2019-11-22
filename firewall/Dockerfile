FROM ubuntu

RUN apt-get update && apt-get upgrade -y && apt-get install -y iputils-ping bridge-utils net-tools iptables

ENTRYPOINT sysctl -w net.ipv4.ip_forward=1 && \
           # iptables -t nat -A PREROUTING  -i eth1 -p tcp -m tcp --dport 80 -j DNAT --to-destination $WORDPRESS_IP && \
           # iptables -t nat -A POSTROUTING -o eth1                          -j SNAT --to-source $FIREWALL_IP && \
           iptables -t nat -A PREROUTING  -p tcp -m tcp --dport 80 -j DNAT --to-destination $WORDPRESS_IP && \
           iptables -t nat -A POSTROUTING                          -j SNAT --to-source $FIREWALL_IP && \
           echo "Done." && \
           /bin/bash
