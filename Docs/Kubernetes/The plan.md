<p>Having a clear plan and defined goals is always advisable. When setting up a Kubernetes cluster, there are certain requirements that must be met. Some of these requirements may be easily fulfilled by using the Turing Pi V2 board, while others may require further decision-making and planning.</p>
<h2>What we need ?</h2>
<p>Every Kubernetes cluster in its very basic form needs just a few things:</p>
<ul>
<li>CPU power</li>
<li>RAM</li>
<li>Storage
<ul>
<li>Persistent storage for apps</li>
</ul>
</li>
<li>Networking</li>
</ul>
<h3>CPU and RAM</h3>
<p>It is important to note that the resource demands of our applications should not exceed the capacity of a single cluster node. This means that if one of the nodes in the cluster only has 2 GB of RAM, like the Raspberry Pi 2 GB RAM modules, the applications should not require more than 2 GB of RAM. Despite the impressive capabilities of Kubernetes, it cannot merge two 2 GB RAM nodes into a single 4 GB RAM cluster. When considering hardware or running applications, it's important to keep in mind that having more RAM is typically more beneficial than having more CPU power.</p>
<h3>Storage</h3>
<p>Storage is often an overlooked aspect in setting up a Kubernetes cluster, yet it is crucial for applications that require persistence of data. If you plan to run applications that need to persist their state, proper storage configuration is a must. On the other hand, if your applications are stateless and do not require data retention between restarts, the storage aspect can be omitted.</p>
<p>In our scenario, we have two reliable storage options:</p>
<ul>
<li><a href="https://longhorn.io/docs" target="_blank" rel="noopener noreferrer">Longhorn</a></li>
<li><a href="https://github.com/kubernetes-csi/csi-driver-nfs" target="_blank" rel="noopener noreferrer">NFS plugin</a></li>
</ul>
<p>While there are other storage options available, these two have proven to be effective solutions. Longhorn requires a minimum of 3 nodes with hard drives in order to establish a storage pool. Using the SD card on Raspberry Pi is not recommended for this purpose (although possible). Instead, we can utilize the onboard SATA connectors and two mini PCIe slots to achieve 3 nodes with SATA SSDs for proper storage. You can find mini PCIe to SATA converters on websites such as AliExpress.</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9041211276061" alt="minipcie_to_sata.png" width="414" height="349"></p>
<p>The most straightforward solution, with fewer added features, is the NFS plugin. With this option, you will only need a single disk attached to the onboard SATA connector, and we will designate Node3 as the NFS server. Although this option is easy to set up, it also has a single point of failure.</p>
<h3>Networking</h3>
<p>Networking in a Kubernetes cluster can be a complex topic, particularly when it comes to network plugins (CNI) and related components once the cluster is up and running. However, the underlying network resources can be relatively straightforward, especially if you are using the Turing Pi V2 board, which provides a 1Gbps network connection between each node. It's important to note that when using persistent storage, the network speed can become a bottleneck, as the application may need to access the disk on a different node than where it is running. To ensure proper communication between the nodes in our Kubernetes cluster, we need to make sure that the build in switch is connected to a router and each node is able to receive an IP address. This can be accomplished by either connecting the Turing Pi V2 directly to your existing network or by using one of the nodes as a router.</p>
<p> </p>
<h2>What we will achieve.</h2>
<p>We will construct a 4-node Kubernetes cluster using 4 Raspberry Pi CM modules. The network setup, persistent storage, and a sample application utilizing persistent storage will be established. This will provide a foundational Kubernetes platform for further exploration and development.</p>
<p>We will be using this schema:</p>
<ul>
<li>Node1 - 10.0.0.60 - <strong>Kubernetes controller</strong> and worker node</li>
<li>Node2 - 10.0.0.61 - Kubernetes worker node</li>
<li>Node3 - 10.0.0.62 - Kubernetes worker node and <strong>NFS server</strong>
</li>
<li>Node4 - 10.0.0.63 - Kubernetes worker node</li>
</ul>
<p>Before setting up Kubernetes, it is important to adjust the network settings to match your network's configuration. In the storage section, we will provide instructions for both NFS and Longhorn setup, however it is recommended to only choose one storage solution. Lastly, as an optional task, we will replace one of the nodes with an Nvidia Jetson module and aim to use it for machine learning tasks.</p>
<p><em><strong>We would be interested in learning about your Kubernetes projects utilizing the Turing Pi V2, so please feel free to share in the comments section.</strong></em></p>
<p> </p>