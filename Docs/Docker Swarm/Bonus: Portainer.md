<p>Portainer is an open-source management UI for Docker that allows you to manage containers, images, networks, and volumes from a web interface. It is a lightweight and simple solution for managing Docker resources.</p>
<p>In a Docker Swarm environment, Portainer can be used to manage multiple nodes and monitor the status of containers, services, and tasks in real-time. It provides a centralized interface to manage the swarm, making it easier to monitor, deploy, and manage Docker containers and services.</p>
<p>Additionally, Portainer provides detailed information about resources, such as memory usage and network statistics, which can be useful for troubleshooting and performance optimization.</p>
<h2>Install</h2>
<p><strong>It is important to note</strong> that during the installation process, we encountered a<strong> failure due to the missing folder /var/lib/docker/volumes.</strong> To prevent this issue from occurring in the future, it is recommended to run the following command on every node in your system:</p>
<pre>mkdir -p /var/lib/docker/volumes</pre>
<p><strong>On your main node</strong>:</p>
<pre># look for up-to-date manifest link here: https://docs.portainer.io/start/install/server/swarm/linux<br>curl -L https://downloads.portainer.io/ce2-16/portainer-agent-stack.yml -o portainer-agent-stack.yml</pre>
<p>To ensure that the data stored by Portainer remains persistent, we can leverage the GlusterFS that has been deployed and mounted on all of our nodes at the location /mnt/docker-storage. This will allow us to store the data in a centralized and highly available manner, ensuring that it is not lost even in the event of a node failure.</p>
<p>Create folder <strong>on primary node:</strong></p>
<pre>mkdir -p /mnt/docker-storage/portainer</pre>
<p><strong>Edit</strong> the<strong> portainer-agent-stack.yml</strong></p>
<p>Down at the bottom, remove:</p>
<pre>volumes:<br>  portainer_data:</pre>
<p>And in Section services -&gt; portainer -&gt; volumes, change it to:</p>
<pre>    volumes:<br>      - type: bind<br>        source: /mnt/docker-storage/portainer<br>        target: /data</pre>
<p>Safe and apply the stack.</p>
<pre>docker stack deploy -c portainer-agent-stack.yml portainer</pre>
<p>Wait for the service to be deployed, you can check it with:</p>
<pre>root@cube01:~# docker service ls<br>ID           NAME                  MODE       REPLICAS IMAGE PORTS<br>c0mqercgib50 portainer_agent       global     4/4      portainer/agent:2.16.2 <br>dak8d8jlkbnp portainer_portainer   replicated 1/1      portainer/portainer-ce:2.16.2 *:8000-&gt;8000/tcp, *:9000-&gt;9000/tcp, *:9443-&gt;9443/tcp</pre>
<h2>Check</h2>
<p>You can access Portainersa web-based graphical user interface for managing Docker Swarm, by navigating to the virtual IP address 10.0.0.70 on port 9000. Once you have reached the setup page, you can set your username and password to securely log in. With these credentials, you can manage your Swarm from a convenient and user-friendly web interface. You can also use any IP address of a node in your Swarm instead of the virtual IP address.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9142075454109" alt="portainer_1.png"></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9142081763485" alt="portainer_2.png"></p>
<h2>Cleanup</h2>
<p>To remove the Portainer stack, you can use command:</p>
<pre>docker stack rm portainer</pre>