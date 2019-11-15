FROM ubuntu

# Allow HTTP (TCP 80) only.
# This blocks DNS, which might be an issue.
RUN apt-get update && apt-get upgrade -y && apt-get install -y bridge-utils net-tools iptables

ENTRYPOINT iptables -A FORWARD -p tcp --dport 80 -j ACCEPT && iptables -A FORWARD -j DROP && /bin/bash
# iptables -t nat -A PREROUTING -p tcp -i --dport 80 -j DNAT --to-destination $WORDPRESS_HOST
