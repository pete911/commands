# networking

## tunnel/port foward

 - ssh (fixed destination) `ssh -Ng -L <local-port>:<remote-host>:<remote-port> <bastion-user>@<bastion-host>`
 - ssh (dynamic destination) `ssh -Ng -D <local-port> <bastion-user>@<bastion-host>`

To use dynamic proxy with ssh (e.g. we need to execute multiple commands but ssh goes over bastion)
 - in one terminal `ssh -Ng -D <local-port> <bastion-user>@<bastion-host>`
 - second terminal `ssh -t -q -o ProxyCommand='nc -x 127.0.0.1:<local-port> %h %p' <user>@<host> '<command>'`

## list active sockets
 - tcp ~~`netstat -avnp tcp`~~ `ss -tnp` or listening `ss -tnlp`
 - udp ~~`netstat -avnp udp`~~ `ss -unp` or listening `ss -unlp`
 - unix ~~`netstat -af unix`~~ `ss -xnp`
 - show processes using tcp `for pid in $(netstat -avnp tcp | tail -n +3 | awk '{print $9}' | uniq);do;ps -p $pid | tail -n +2;done`
 - show processes using upd `for pid in $(netstat -avnp udp | tail -n +3 | awk '{print $8}' | uniq);do;ps -p $pid | tail -n +2;done`
 - `ss -s` socket summary `-a` - all sockets, `-u -a` - upd sockets, `-t -a` - tcp sockets
 - `cat /proc/net/tcp` - list of open tcp sockets
 - `cat /proc/net/udp` - list of open udp sockets
 - `cat /proc/net/raw` - list of open raw sockets
 - https://gist.github.com/jkstill/5095725 `/proc/net/tcp`
 - [max number of tcp/ip connections](https://stackoverflow.com/questions/410616/increasing-the-maximum-number-of-tcp-ip-connections-in-linux)

Sockets can be used with curl e.g. `curl --unix-socket /var/run/docker.sock localhost/v1.40/images/json | jq .` (localhost is just a
dummy hostname required by curl). To check if the file is a socket run `file <file>` or `ls -ld <file>` and check first character.

## show MAC address of remote host
 - `arp <IP/host>`

## show all devices on the local network
 - ping broadcast `ping 255.255.255`
 - show arp cache `arp -a`

or
 - `namp -sn <CIDR>` e.g. `nmap -sn 192.168.1.1/24`

## IP changed MAC it is at
Send ARP to response to braodcast MAC (FF:FF:FF:FF:FF:FF) 'Gratuitous ARP' <IP> is on <interface>:
 - mac os `arping -PU -i <interface> <IP>` e.g. `arping -PU -i eth0 172.16.42.161`
 - linux `arping -A -I <interface> <IP>` e.g. `arping -A -I eth0 172.16.42.161`

## connections tracking table
 - show connections `cat /proc/net/nf_conntrack` (router/server NAT/SNAT, netfilter, ...)
 - `contrack -L` [netfilter](https://netfilter.org/projects/conntrack-tools/index.html)

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
 - `netstat -nr` for mac users
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
