FROM ubuntu

RUN apt-get update && apt-get upgrade -y && apt-get install -y iputils-ping


# This will ping indefinitely all comma-separated IP addresses from list variable ADDRS and print a report if it could or couldn't reach each one of these addresses 

ENTRYPOINT while [ True ]; do \
                for addr in $(echo $ADDRS | sed "s/,/ /g"); do \
                    status=$(echo $(ping $addr -c 1 -w 1 | grep 'received' | cut -d ',' -f 2 | cut -d ' ' -f 2)) && \
                    if test $status = 0 ; then \
                        echo "Failed to reach $addr"; \
                        else echo "Sucessfully reached $addr"; \
                    fi; done; \ 
                echo ""; \
                sleep 1; \
            done && \
           /bin/bash

# r/AbsoluteUnits

