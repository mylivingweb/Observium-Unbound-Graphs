# Observium-Unbound-Graphs
working config for Observium to graph unbound DNS queries
###Prerequisites
working Observium installation http://observium.org
working Unbound installation https://unbound.net
networking between the 2 servers

####Working Config (Tested on 2 CentOS 7 boxes)
#####On Unbound Server
install xinetd
Copy observium_xinetd to xinetd folder
```
cp observium_xinetd to /etc/xinetd.d/
```
Make dirs for unbound file
```
mkdir -p /usr/lib/observium_agent/local/
```
Copy and change permissions of unbound file
```
cp unbound /usr/lib/observium_agent/local/
chmod +x /usr/lib/observium_agent/local/unbound
```
FROM your Observium Server 

```scp /opt/observium/scripts/observium_agent user@YOUR_Unbound_Server:/usr/bin/observium_agent ```

FROM your Observium Server (optional) 

```scp /opt/observium/scripts/distro user@YOUR_Unbound_server:/usr/bin/distro ```
```echo "extend .1.3.6.1.4.1.2021.7890.1 distro /usr/bin/distro" >> /etc/snmp/snmpd.conf```
restart snmpd
start xinetd

Edit unbound.conf to enable stats

```
statistics-cumulative: yes
extended-statistics: yes
```
restart unbound

#####On Observium Server
*This is working for me but may not be the best way

```
mkdir -p /usr/lib/observium_agent/local/
```

Sym Link unbound and observium_agent to new directory

```
ln -s /opt/observium/scripts/agent-local/unbound /usr/lib/observium_agent/local/unbound
ln -s /opt/observium/scripts/observium_agent /usr/bin/observium_agent
```

test xinetd by using ```telnet Unbound_Server_IP 36602```

####If all is working
In webui of Observium, either edit your server or add a new server and enable unix agent

If existing server, go to server overview page and click the gear and go to modules, make sure unix agent is enabled

Wait a few polling sessions for graphs to show
