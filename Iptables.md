## Understanding iptables: The Linux Firewall Essentials
iptables is a powerful, user-space command-line tool used to configure and manage the Linux kernel's netfilter framework, which handles packet filtering, NAT (Network Address Translation), and packet mangling. It has been the standard firewall tool in Linux for over two decades and is still widely used, even though nftables is its official successor.

### Key Concepts
iptables works with four main built-in tables:

filter – The default table for packet filtering (allowing, dropping, or rejecting packets).
nat – Used for Network Address Translation (SNAT, DNAT, MASQUERADE).
mangle – For specialized packet alteration (e.g., modifying ToS, TTL).
raw – For marking packets to bypass connection tracking.

Within each table, rules are organized into chains:

INPUT – Packets coming into the local system
FORWARD – Packets being routed through the system
OUTPUT – Packets originating from the local system
PREROUTING (nat/mangle/raw) – Before routing decision
POSTROUTING (nat/mangle) – After routing decision

### Basic Rule Structure
A typical iptables rule looks like this:
```
#command
iptables -t filter -A INPUT -s 192.168.1.100 -j DROP
```
 * -t specifies the table (default: filter)
 * -A appends the rule to a chain
 * -s matches source IP
 * -j specifies the target (ACCEPT, DROP, REJECT, LOG, etc.)

```
# Allow established/related connections
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Allow SSH from a specific IP
iptables -A INPUT -p tcp --dport 22 -s 203.0.113.50 -j ACCEPT

# Allow loopback traffic
iptables -A INPUT -i lo -j ACCEPT

# Drop everything else (default policy should also be DROP)
iptables -P INPUT DROP

# Enable forwarding and NAT (simple router)
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

### Persistence
Rules added with iptables are lost on reboot. Common ways to make them persistent:

iptables-save > /etc/iptables/rules.v4 and restore on boot
Use tools like iptables-persistent, firewalld, or ufw (which are frontends)
