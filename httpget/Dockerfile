FROM ubuntu

RUN apt-get update && apt-get upgrade -y && apt-get install -y wget

# This will attempts to retrieve web page using wget from all comma-separated IP addresses of the list variable ADDRS and print a report if it could or couldn't get the page.


ENTRYPOINT while [ True ]; do \
                for addr in $(echo $ADDRS | sed "s/,/ /g"); do \
                    wget $addr -T 1 -t 1 -q -O /dev/null; status=$(echo $?) && \
                    if test $status = 0 ; then \
                        echo "Sucessfully retrieved page from $addr"; \
                        else echo "Failed to retrieve page from $addr"; \
                    fi; \
                done; \
                echo ""; \ 
                sleep 1; \
            done && \
           /bin/bash
