<p>Having a well-defined plan and clear objectives is crucial, regardless of whether you are setting up a Kubernetes cluster or a simpler solution like Docker Swarm. Both systems have specific requirements that need to be met, and choosing the right hardware solution can greatly ease this process. In particular, the Turing Pi V2 board may offer a convenient and effective solution for fulfilling some of these requirements.</p>
<h1>Docker Swarm</h1>
<p>Docker Swarm is a native orchestration system for Docker containers. It allows users to manage and deploy multiple containers as a single system. Swarm uses the same API as Docker, making it familiar and easy to use for those already familiar with Docker.</p>
<p>On the other hand, Kubernetes is a more feature-rich and complex container orchestration system that was originally developed by Google. While Kubernetes provides a lot of features and is considered to be the industry standard, it has a steeper learning curve and may be overkill for smaller, simpler deployments.</p>
<p>Therefore, someone might prefer <strong>Docker Swarm</strong> over Kubernetes for its simplicity, ease of use, and <strong>lower overhead</strong> for smaller scale deployments. On the other hand, if you need advanced features and are running a large-scale deployment, Kubernetes might be the better choice.</p>
<h2>OS</h2>
<p>Once more, we will utilize <a href="https://dietpi.com/" target="_blank" rel="noopener noreferrer"><strong>DietPi</strong> </a>for its ease of setup. With DietPi, we can have our system up and running with minimal effort and without the need for extensive manual configuration.</p>
<h2>Network</h2>
<p>Swarm offers several network drivers that determine how the network is created and how containers are connected to it. The most common network driver is the "bridge" driver, which creates a virtual network inside the Docker host and connects containers to it.</p>
<p>When you export your docker service in Docker Swarm, any Node in swarm can pick up the connection and relay it to the correct container. This is fine in general, however for our convenience we can use <strong><a href="https://www.keepalived.org/" target="_blank" rel="noopener noreferrer">Keepalived</a>.</strong></p>
<h3>Keepalived</h3>
<p>Keepalived is a high availability solution that monitors the health of servers in a network and automatically redirects traffic to healthy servers in case of failure. This ensures that the services provided by the servers are always available and minimizes downtime.</p>
<p>In simple terms, Keepalived is a tool that helps keep a service or application running by detecting when a server is not working and redirecting traffic to another server that is working. This helps ensure that the service or application is always available to users.</p>
<p>In our case, we will create one virtual IP that will serve as our single point of entry. This IP will always point to one of the Nodes and if one Node fails, the IP will renegotiate its routing automatically and always point to the other swarm node.</p>
<h3>Layout</h3>
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
<strong>Ingress</strong> (virtual IP) - 10.0.0.70</li>
</ul>
<h2>Storage</h2>
<p>While Docker Swarm does not have a built-in persistent storage feature that can handle the migration of containers between nodes, there is a third-party solution available. This solution is called <strong><a href="https://www.gluster.org/" target="_blank" rel="noopener noreferrer">GlusterFS</a></strong>, and it provides a robust and flexible option for persistent storage in a Docker Swarm environment.</p>
<h3>GlusterFS</h3>
<p>GlusterFS is a scalable, distributed file system that allows you to store and access files across multiple servers as if they were on a single server. It provides features such as automatic data replication and data distribution across multiple nodes, allowing you to store large amounts of data and ensure high availability and data redundancy.</p>
<p>Compared to NFS (Network File System), GlusterFS has several advantages. GlusterFS offers better scalability, as it can easily grow and accommodate increasing storage needs by adding more nodes to the cluster. GlusterFS also provides a higher level of data protection, as it automatically replicates data across multiple nodes, reducing the risk of data loss. Additionally, GlusterFS can distribute data across nodes for improved performance, making it well-suited for high-performance storage requirements.</p>
<p>In summary, GlusterFS provides better scalability, data protection, and performance compared to NFS, making it a more suitable solution for enterprise-level storage needs.</p>
<p>While it is possible to configure GlusterFS with just one replica, which would result in data being stored on a single node, this approach is not recommended. To ensure high availability and data protection, it is advisable to have at least two replicas in a GlusterFS cluster.</p>
<p>In terms of storage hardware, using mini PCIe SATA cards and connecting SSD disks to three nodes is a recommended option. This provides a robust and reliable storage solution for GlusterFS. However, it's important to note that SD cards are not suitable for heavy writing and reading environments, and should not be used as the primary storage backend for GlusterFS or a Kubernetes cluster.</p>