# networking

## tunnel/port foward

 - ssh (fixed destination) `ssh -Ng -L <local-port>:<remote-host>:<remote-port> <bastion-user>@<bastion-host>`
 - ssh (dynamic destination) `ssh -Ng -D <local-port> <bastion-user>@<bastion-host>`

## list active sockets
 - tcp `netstat -avnp tcp`
 - udp `netstat -avnp udp`
 - show processes using tcp `for pid in $(netstat -avnp tcp | tail -n +3 | awk '{print $9}' | uniq);do;ps -p $pid | tail -n +2;done`
 - show processes using upd `for pid in $(netstat -avnp udp | tail -n +3 | awk '{print $8}' | uniq);do;ps -p $pid | tail -n +2;done`

## arp
 - show arp cache `arp -l`

## connections tracking table
 - show connections `cat /proc/net/nf_conntrack` (router/server NAT/SNAT, netfilter, ...)

## ip tables
List
 - all rules by specification `sudo iptables -S`
 - specific rule by specification `sudo iptables -S TCP`
 - all rules sorted by chain `sudo iptables -L`
 - specific chain `sudo iptables -L INPUT`

Options
 - `-t` table [filter, nat, mangle, raw], defaults to filter
 - `-L` list all the rules in selected chain, if chain is not specified all chains are listed
 - `-S` similar to `-L`, but all chains are printed like iptables-save
 - `-Z` zero the packet and byte counters in all chains (can be specified with -L)
 - `-v` verbose
 - `-n` numeric, by default, the program will try to display them as host names, network names, or services
 - `-x` expand numbers, display the exact value of the packet and byte counters
 - `--line-numbers` add line numbers to the beginning of each rule, corresponding to that rule's position in the chain

### watch iptables traffic
In one terminal watch the chain (you can reset counters with `-Z` option before running the command)
 - specific chain `watch -d 'sudo iptables -t nat -L <chain> -nvx'` or `watch 'sudo iptables -t nat -S <chain> -v'`
 - all chains `watch -d 'sudo iptables -t nat -v -S | grep -e ^\-A'`
In second terminal (on the same node) create load
 - `watch -n1 'curl -s <IP|host> > /dev/null'`

## routing
 - `ip addr` display ip addresses and property information
 - `ip addr show dev <interface>` display info only for the interface
 - `ip link [show dev <interface>]` display the state of all network interfaces
 - `ip route` list all of the route entries in the kernel (does not show dummy interfaces e.g. kube-ipvs0)
 - `ip route get` display the route an address will take
 - `ip route show table local` show local route table (dummy interfaces as well)

### monitor traffic
 - `route -n` or `ip route show`
 - get interface for destination
 - run tcpdump `sudo tcpdump -n -i <interface>`

### tcpdump
 - `sudo tcpdump -i eth0 -n` -i <interface>, -n do not resolve IP addresses using reverse DNS
 - `sudo tcpdump -i eth0 -n net 10.16.0.0/16` CIDR block
 - `sudo tcpdump -i eth0 -n [src|dst] net 10.16.0.0/16` src|dst CIDR block
 - `sudo tcpdump -i eth0 -n [src|dst] host 192.168.1.100` host, src|dst host
 - `sudo tcpdump -i eth0 -n [tcp] port 80` port
 - `sudo tcpdump -i eth0 -n host 192.168.1.11 or host 192.168.1.15 and tcp port 80` and, or, not ...
 
 ## dns
  - dig show only answer `dig +noall +answer <host-name>`
