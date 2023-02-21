<p>We previously discussed <strong>Keepalived</strong> in our planning section, but to summarize, it enables us to use a single, shared virtual IP to access our service on Docker Swarm. This eliminates the need to target individual nodes and instead allows us to target one virtual IP that is self-healing and highly available. By doing so, we no longer require an external load balancer.</p>
<h2>Our layout:</h2>
<ul>
<li>
<strong>Node1</strong> - 10.0.0.60</li>
<li>
<strong>Node2</strong> - 10.0.0.61</li>
<li>
<strong>Node3</strong> - 10.0.0.62</li>
<li>
<strong>Node4</strong> - 10.0.0.63</li>
<li>
<strong>Ingress</strong> (virtual IP - <strong>Keepalived</strong>) - 10.0.0.70</li>
</ul>
<h2>Install Keepalived</h2>
<p><strong>On every node:</strong></p>
<pre>apt-get -y install keepalived</pre>
<p><strong>On master node</strong> (cube01) create file "/etc/keepalived/keepalived.conf"</p>
<pre>global_defs {<br>  router_id DOCKER_INGRESS<br>}<br><br>vrrp_instance VI_1 {<br>  state MASTER<br>  interface eth0<br>  virtual_router_id 51<br>  priority 100<br>  advert_int 1<br>  authentication {<br>    auth_type PASS<br>    auth_pass mypassword<br>  }<br>  virtual_ipaddress {<br>  10.0.0.70<br>  }<br>}<br><br></pre>
<p>Keepalived configuration file consists of various parameters that define how the virtual IP address should be managed and maintained in a high availability environment. Some of the common parameters in Keepalived's configuration file include:</p>
<ul>
<li>
<strong>router_id</strong>: is a unique identifier for the keepalived instance. The identifier is used by keepalived to differentiate between multiple instances of the service running on the same system. The value of the router_id can be any string that is unique to the keepalived instance.</li>
<li>
<strong>vrrp_instance</strong>: defines the virtual router instance that will manage the virtual IP address.</li>
<li>
<strong>interface</strong>: specifies the network interface that will be used to manage the virtual IP address.</li>
<li>
<strong>state</strong>: defines the initial state of the virtual IP address, which can be either MASTER or BACKUP.</li>
<li>
<strong>virtual_router_id</strong>: a unique identifier for the virtual router instance.</li>
<li>
<strong>priority</strong>: defines the priority of the node to take over the virtual IP address. A higher value indicates a higher priority.</li>
<li>
<strong>virtual_ipaddress</strong>: lists the virtual IP addresses that should be assigned to the node with the highest priority.</li>
<li>
<strong>advert_in</strong>: refers to the advertisement interval. It specifies the frequency in seconds at which a keepalived instance sends gratuitous ARP announcements to update its local ARP cache table on the network.</li>
<li>
<strong>auth_pass:</strong> is the authentication password used between the keepalived instances running on different nodes. It is used to prevent unauthorized access to the keepalived service, and to ensure that only authorized keepalived instances can modify the virtual IP configuration.</li>
</ul>
<p><strong>On Node2 same file "/etc/keepalived/keepalived.conf"</strong></p>
<pre>global_defs {<br>  router_id DOCKER_INGRESS<br>}<br><br>vrrp_instance VI_1 {<br>  state BACKUP<br>  interface eth0<br>  virtual_router_id 51<br>  priority 90<br>  advert_int 1<br>  authentication {<br>    auth_type PASS<br>    auth_pass mypassword<br>  }<br>  virtual_ipaddress {<br>    10.0.0.70<br>  }<br>}</pre>
<p>We only changed two things:</p>
<ul>
<li>
<strong>state</strong> to BACKUP</li>
<li>
<strong>priority</strong> to 90 from 100</li>
</ul>
<p><strong>To configure nodes 3 and 4</strong>, you'll need to make a similar configuration as the node 2. But <strong>decrease the priority of each node</strong> by 10. For example, node 3 would have a priority of 80, and node 4 would have a priority of 70.</p>
<p><strong>Start and enabled the service</strong> to start at boot<strong> on every node</strong> (start from Node 1)</p>
<pre>systemctl start keepalived<br>systemctl enable keepalive</pre>
<h2>Check</h2>
<p>To check if keepalived successfully negotiated the virtual IP, you can use the <code>ip a</code> command to list the IP addresses assigned to the network interfaces. The virtual IP should be listed under the interface specified in the <code>interface</code> directive in keepalived.conf and the state should be "MASTER" on the node with the highest priority, and "BACKUP" on the other nodes. You can also check the logs in the <code>/var/log/syslog</code> or <code>/var/log/messages</code> for any error messages related to keepalived.</p>
<pre>root@cube01:~# ip a show eth0<br>2: eth0: &lt;BROADCAST,MULTICAST,UP,LOWER_UP&gt; mtu 1500 qdisc mq state UP group default qlen 1000<br>    link/ether e4:5f:01:b7:4d:9e brd ff:ff:ff:ff:ff:ff<br>    inet 10.0.0.60/24 brd 10.0.0.255 scope global eth0<br>       valid_lft forever preferred_lft forever<br>    inet 10.0.0.70/32 scope global eth0<br>       valid_lft forever preferred_lft forever</pre>
<p>You can also try to ping the virtual IP from other nodes.</p>
<pre>root@cube04:~# ping -c 3 10.0.0.70<br>PING 10.0.0.70 (10.0.0.70) 56(84) bytes of data.<br>64 bytes from 10.0.0.70: icmp_seq=1 ttl=64 time=0.271 ms<br>64 bytes from 10.0.0.70: icmp_seq=2 ttl=64 time=0.225 ms<br>64 bytes from 10.0.0.70: icmp_seq=3 ttl=64 time=0.199 ms<br><br>--- 10.0.0.70 ping statistics ---<br>3 packets transmitted, 3 received, 0% packet loss, time 2038ms<br>rtt min/avg/max/mdev = 0.199/0.231/0.271/0.029 ms</pre>