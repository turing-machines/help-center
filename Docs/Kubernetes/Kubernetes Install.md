<p>We have chosen to use K3s as the Kubernetes flavor for our setup, as it offers a complete Kubernetes experience while requiring fewer resources in terms of CPU and RAM compared to other options available.</p>
<p>With your favorite ssh client, <strong>log into Node1. </strong></p>
<p>For our reference, we will use this table again:</p>
<ul>
<li>
<strong>Node1</strong> - 10.0.0.60 - <strong>Kubernetes controller</strong> and worker node</li>
<li>
<strong>Node2</strong> - 10.0.0.61 - Kubernetes worker node</li>
<li>
<strong>Node3</strong> - 10.0.0.62 - Kubernetes worker node and <strong>NFS server</strong>
</li>
<li>
<strong>Node4</strong> - 10.0.0.63 - Kubernetes worker node</li>
</ul>
<h2>Initializing Master Node</h2>
<pre>curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --disable servicelb --token myrandompassword --node-ip 10.0.0.60 --disable-cloud-controller --disable local-storage</pre>
<p><strong>Some explanations:</strong></p>
<ul>
<li>
<strong>--write-kubeconfig-mode 644</strong> - This flag is used to specify the mode for the kubeconfig file. It is optional, but required if you plan to connect to the Rancher manager later.</li>
<li>-<strong>-disable servicelb</strong> - This flag disables the service load balancer. Instead, we will use metallb.</li>
<li>
<strong>--token</strong> - This flag sets the token used to connect to the K3s master node. Choose a secure random password and keep it safe.</li>
<li>
<strong>--node-ip</strong> - This flag sets the K3s master node to bind to a specific IP address.</li>
<li>
<strong>--disable-cloud-controller</strong> - This flag disables the K3s cloud controller, which we deemed unnecessary in our use case.</li>
<li>
<strong>--disable local-storage</strong> - This flag disables the K3s local storage, as we will be setting up the longhorn storage provider and NFS provider instead.</li>
</ul>
<p>After the installation is complete, you can verify its success by using the command "<strong>kubectl get nodes</strong>". This will return output similar to the following:</p>
<pre>root@cube01:~# kubectl get nodes<br>NAME   STATUS ROLES                AGE    VERSION<br>cube01 Ready control-plane,master 6m22s v1.25.6+k3s1</pre>
<h2>Adding Workers</h2>
<p>Next we will add the rest of the workers. Log in via SSH to Node2 to 4 and execute the following command.</p>
<pre>curl -sfL https://get.k3s.io | K3S_URL=https://10.0.0.60:6443 K3S_TOKEN=myrandompassword sh -</pre>
<p>You do not have to do it one by one, you can execute the command on rest of the nodes simultaneously.</p>
<p>When the script finishes, <strong>on Node1</strong> execute again "<strong>kubectl get nodes</strong>". This will return output similar to the following:</p>
<pre>root@cube01:~# kubectl get nodes<br>NAME   STATUS ROLES  AGE  VERSION<br>cube01 Ready control-plane,master 43m v1.25.6+k3s1<br>cube04 Ready &lt;none&gt; 38s v1.25.6+k3s1<br>cube02 Ready &lt;none&gt; 35s v1.25.6+k3s1<br>cube03 Ready &lt;none&gt; 34s v1.25.6+k3s1</pre>
<h2>Labels</h2>
<p>This step is optional, but it is recommended to label/tag the nodes in order to see the role as "worker" instead of "&lt;none&gt;". Labeling the nodes is not just a cosmetic change, it can be useful in specifying a node for running certain workloads. For instance, if you have a node with specialized hardware, such as a Jetson Nano, you can label that node and direct applications that require that hardware to run only on that node.</p>
<p>Let's add this tag key:value: kubernetes.io/role=worker to nodes. This is more cosmetic, to have nice output from kubectl get nodes.</p>
<pre>kubectl label nodes cube01 kubernetes.io/role=worker<br>kubectl label nodes cube02 kubernetes.io/role=worker<br>kubectl label nodes cube03 kubernetes.io/role=worker<br>kubectl label nodes cube04 kubernetes.io/role=worker</pre>
<p>There is another label/tag that can be added. This label can be used to specify a preference for nodes with "node-type=workers" for running deployments. "node-type" is the chosen key name, but it can be named anything.</p>
<pre>kubectl label nodes cube01 node-type=worker<br>kubectl label nodes cube02 node-type=worker<br>kubectl label nodes cube03 node-type=worker<br>kubectl label nodes cube04 node-type=worker</pre>
<p>If your cube04 is Nvidia Jetson, you could use node-type=jetson, and use this to run ML containers only on that node.</p>
<p>Your nodes should look like this now:</p>
<pre>root@cube01:~# kubectl get nodes<br>NAME   STATUS ROLES AGE VERSION<br>cube01 Ready control-plane,master,worker 52m v1.25.6+k3s1<br>cube02 Ready worker 10m v1.25.6+k3s1<br>cube03 Ready worker 10m v1.25.6+k3s1<br>cube04 Ready worker 10m v1.25.6+k3s1</pre>
<p>You can list all tags per node with:</p>
<pre>kubectl get nodes --show-labels</pre>
<p><span class="wysiwyg-color-green110"><strong>Congratulations, you have successfully set up a Kubernetes cluster! However, there are still additional steps to take to enhance the usability of the cluster. Keep reading our guide for next steps.<br></strong></span></p>
<p> </p>