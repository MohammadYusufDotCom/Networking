### How to check all incoming traffic using tcpdump
```
tcpdump -i ens5 'tcp[tcpflags] & tcp-syn != 0 and not tcp[tcpflags] & tcp-ack != 0' -vvv

# -nn for converting traffic DNS to IP
# -vvv for verbose mode
# Rest is for checking traffic which is originated from outside of instance
```
#### In ouptput
```

'Flags [S]'

Means 
Flaf    Meaning	  Purpose
S	      SYN	      start TCP connection
.	      ACK	      acknowledgement
P	      PUSH	    contains data
F	      FIN	      close connection
R	      RST	      reset connection
```
