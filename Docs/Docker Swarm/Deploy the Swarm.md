<p><strong>On your master node</strong>, which in this case is Node1 with IP address 10.0.0.60 and hostname cube01, the process for initializing the Docker Swarm is straightforward.</p>
<p>Simply log in as the root user and run the following commands to complete the process.</p>
<pre>docker swarm init --advertise-addr 10.0.0.60</pre>
<p>It should return us output similar to this:</p>
<pre>Swarm initialized: current node (myjwx5z3m7kcrplih1yw0e2sy) is now a manager.<br><br>To add a worker to this swarm, run the following command:<br><br>docker swarm join --token SWMTKN-1-2gwqttpup0hllec6p6xkun8nmht4xu18g09vsxyjhlyqc9sgjw-729yfmz5rfg02eiw0537m49c1 10.0.0.60:2377<br><br>To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.<br><br></pre>
<p>From the output, we have two important things:</p>
<ul>
<li>docker swarm join --token &lt;token&gt; 10.0.0.60:2377</li>
<li>docker swarm join-token manager</li>
</ul>
<p>The <strong>first command</strong> in this output is used to join additional <strong>worker</strong> nodes to your Docker Swarm cluster, while the second command will allow us to print a command to join more <strong>managers. You can have just one manager node, but in our case it's recommended to have 3.</strong> This is for the simple reason that when one manager fails, another manager will be elected as a leader and everything will continue working. Why would we not set 4 ? The number of managers should always be an odd number. More information can be found in official documentation <a href="https://docs.docker.com/engine/swarm/admin_guide/#operate-manager-nodes-in-a-swarm" target="_blank" rel="noopener noreferrer"><strong>HERE.</strong></a></p>
<p>You can always use command:</p>
<pre>docker swarm join-token worker</pre>
<p>To get the original worker token command.</p>
<h2>Join managers</h2>
<p>It is important to note that the role of Master and Worker nodes in Docker Swarm is distinct from their Kubernetes counterparts. In Docker Swarm, any node can be designated as a Master node, while all nodes are considered Worker nodes. This differs from the way these roles are assigned in Kubernetes, and highlights the flexibility and scalability of Docker Swarm.</p>
<p>We will make our Node2 and Node3 also managers:</p>
<pre># Get the join command from current manager node<br>docker swarm join-token manager<br># On Node2 and 3 use that command to join, in our case:<br>docker swarm join --token SWMTKN-1-2gwqttpup0hllec6p6xkun8nmht4xu18g09vsxyjhlyqc9sgjw-eba5cbn1o4zv449w441bndfv0 10.0.0.60:2377</pre>
<h2>Join workers</h2>
<p>On Node4 execute following join command (Take the token from the master node)</p>
<pre>root@cube04:~# docker swarm join --token SWMTKN-1-2gwqttpup0hllec6p6xkun8nmht4xu18g09vsxyjhlyqc9sgjw-729yfmz5rfg02eiw0537m49c1 10.0.0.60:2377<br>This node joined a swarm as a worker.</pre>
<h2>Check</h2>
<p>On <strong>any of the manager node</strong>, run "docker node ls":</p>
<pre><br>root@cube02:~# docker node ls<br>root@cube02:~# docker node ls<br>ID                        HOSTNAME STATUS AVAILABILITY MANAGER STATUS ENGINE VERSION<br>myjwx5z3m7kcrplih1yw0e2sy cube01   Ready  Active       Leader         23.0.0<br>hox556neondt5kloil0norswb * cube02 Ready  Active       Reachable      23.0.0<br>m9a7tbcksnjhm01bs8hyertik cube03   Ready  Active       Reachable      23.0.0<br>ylkhufgruwpq2iafjwsw01h4r cube04   Ready  Active                      23.0.0</pre>
<p>If you are on a worker node, this command will not work. You can easily <strong>find which node is Leader</strong> at the moment with:</p>
<pre>root@cube02:~# docker info | grep -A1 'Manager Addresses'<br>Manager Addresses:<br>10.0.0.60:2377</pre>
<p><strong>To promote a worker node to a leader node</strong> in a Docker Swarm cluster, you can run the following command in the terminal:</p>
<pre>docker node promote &lt;node-name&gt;</pre>
<p>Replace &lt;node-name&gt; with the hostname of the node you want to promote. This command will change the node's role from worker to leader, granting it additional privileges and responsibilities within the swarm. Note that in a Docker Swarm cluster, there can only be one active leader node at a time, and promoting a node to leader will demote the current leader to a worker.</p>
<h2>Network</h2>
<p><strong>During our testing, we had<span class="wysiwyg-color-red"> issue exporting ports</span>. Seems like the overlay ingress network that is automatically created was in the same range as the nodes.</strong></p>
<p><strong>Our Nodes have IPs from 10.0.0.x and the network was set to:</strong></p>
<pre>}<br>"Subnet": "10.0.0.0/8",<br>"Gateway": "10.0.0.1"<br>}</pre>
<p>You can check that with commands:</p>
<pre>root@cube01:~# docker network ls<br>NETWORK ID    NAME             DRIVER         SCOPE<br>c7ba0aae930a  bridge           bridge         local<br>48ce906c3544  docker_gwbridge  bridge         local<br>5c6001c2110e  host             host           local<br>kpiocqticjlx  ingress          overlay        swarm<br>fb28177a7b9a  none             null           local<br>k55h53e1e97d  portainer_agent_network overlay swarmls</pre>
<pre># Using the ID: kpiocqticjlx of ingress network.<br>docker network inspect --format='{{json .IPAM.Config}}' kpiocqticjlx</pre>
<p>If the range is the same as your node IPs, you need to change it.</p>
<pre>docker network rm ingress<br># Create in different range<br>docker network create --driver overlay --ingress --subnet 172.16.0.0/16 --gateway 172.16.0.1 ingress</pre>
<p><span class="wysiwyg-color-red"><strong>Restart all your nodes.</strong></span></p>
<h2>Done ?</h2>
<p>With that, you have successfully set up a Docker Swarm cluster. However, there are still ways to optimize and improve it. To continue learning about these possibilities, read the other sections of the guide. If you have any interesting projects utilizing the Turing Pi V2 board and Docker Swarm, feel free to share in the comment section.</p>