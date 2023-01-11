# Task 5.1

### Linux Networking


![Photo](./screen/Screenshot_15.png)


### *0. In order for packets to pass through our server to other subnets, you need to set the value net.ipv4.ip_forward=1.

> sudo sysctl -w net.ipv4.ip_forward=1

or

> sudo echo 1 > /proc/sys/net/ipv4/ip_forward

Using either method above will not make the change persistent. To make sure the new setting survives a reboot, you need to edit the /etc/sysctl.conf file.

### 1. Configured interfaces on server, created networks 10.2.12.0/24, 10.3.2.0/24, 172.16.12.0/24.

File: ubuntu - /etc/netplan/*.yaml, centos7 - /etc/sysconfig/network-scripts/ifcfg-(name intarfaces)

![Photo](./screen/Screenshot_1.png)

![Photo](./screen/Screenshot_5.png)

### 2. DHCP.

#### Installed Dhcpd.

> apt-get install isc-dhcp-server -y 

> sudo systemctl start isc-dhcp-server.service

> sudo systemctl enable isc-dhcp-server.service


Open /etc/dhcp/dhcpd.conf and write.

![Photo](./screen/Screenshot_2.png)

> systemctl restart isc-dhcp-server.service

Client1:

![Photo](./screen/Screenshot_4.png)

Client2:

![Photo](./screen/Screenshot_3.png)

### 3. Ping.

> ping: 10.2.12.2 to 10.3.2.2

![Photo](./screen/Screenshot_6.png)



>ping 10.2.12.2 to internet(google.com)

![Photo](./screen/Screenshot_7.png)

No connected to internet, because the packets reach the destination, but the router does not know where to send the response.

![Photo](./screen/Screenshot_8.png)


#### Added static routes on router.

![Photo](./screen/Screenshot_router.png)

Сheck traceroute

> traceroute 10.2.12.2 to .8.8.8.8

![Photo](./screen/Screenshot_9.png)


### 4. Add static IP on loopback intarfaces.

![Photo](./screen/Screenshot_10.png)


Created routing client2 to client1.


Sever:

> sudo ip r add 172.17.22.1 via 10.2.12.1 dev enp0s8

![Photo](./screen/Screenshot_11.png)

client2:

in server

> sudo ip r add 172.17.22.1 via 10.3.2.1 dev enp0s3


in net4

> sudo route add -host 172.17.32.1 gw 172.16.12.2

> traceroute:

![Photo](./screen/Screenshot_12.png)

### 5. Summarizing IP.

in server:

> sudo route add -net 172.17.0.0 netmask 255.255.192.0 gw 10.2.12.1 dev enp0s8

![Photo](./screen/Screenshot_14.png)

in client2:

> sudo route add -net 172.17.0.0 netmask 255.255.192.0 gw 10.3.2.1 dev enp0s3

![Photo](./screen/Screenshot_13.png)


### 6. Openssh. ssh-keygen, ssh-copy-id.

Server:

![Photo](./screen/Screenshot_ssh.png)

Client2:

![Photo](./screen/Screenshot_ssh2.png)

Client3:

![Photo](./screen/Screenshot_ss3.png)


### 7. Iptables.

ACCEPT:

> iptables -A INPUT -s 10.3.2.0/24 -p tcp --dport 22 -j DROP

DROP:

> iptables -A INPUT -s 10.2.12.0/24 -p tcp --dport 22 -j ACCEPT

> iptables -A FORWARD -s 10.3.2.2 -d 172.17.32.1 -p icmp -j DROP

> iptables -A FORWARD -s 10.3.2.2 -d 172.17.22.1 -p icmp -j DROP

![Photo](./screen/Screenshot_iptables.png)

### 8. NAT

> iptables -A POSTROUTING -t nat -s 10.3.2.0/24 -o enp0s3 -j MASQUERADE

> iptables -A POSTROUTING -t nat -s 10.2.12.0/24 -o enp0s3 -j MASQUERADE

OR

> iptables -A POSTROUTING -t nat -s 10.3.2.0/24 -o enp0s3 -j SNAT --to-source 192.168.178.75

> iptables -A POSTROUTING -t nat -s 10.3.12.0/24 -o enp0s3 -j SNAT --to-source 192.168.178.75


![Photo](./screen/Screenshot_nat.png)
