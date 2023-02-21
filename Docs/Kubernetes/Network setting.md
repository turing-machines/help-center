<p>Networking in Kubernetes can be a complex and multifaceted challenge. It involves considering various networking scenarios, such as how containers will communicate with one another internally (Internal Networking), and how services will be made accessible to users outside of the cluster (External Networking). This can involve setting up routing rules, configuring load balancers, and deciding on appropriate DNS or IP addresses. Ensuring a robust and secure network architecture is crucial for the proper functioning of a Kubernetes cluster.</p>
<p> </p>
<p>In order to keep the networking aspect of Kubernetes simple and contained within the cluster, we will opt for an internal solution rather than using external DNS servers or load balancers. This means that we will assign IP addresses from our network to services and allow them to be accessed through their own IPs, resulting in a more straightforward and manageable setup.</p>
<p><strong>We are going to use HELM in this example, so please read the HELM guide first.</strong></p>
<p> </p>
<h2>MetalLB</h2>
<p>You can read what exactly MetalLB does in great detail on their own home page <a href="https://metallb.universe.tf/" target="_blank" rel="noopener">HERE.</a></p>
<p>MetalLB is a Kubernetes-based load balancer that provides load balancing services for services running within a cluster. It operates by assigning IP addresses to services, and responding to network requests to those IPs. This enables services to be exposed externally to clients, making them more accessible and scalable. It is useful in cases where a Kubernetes cluster does not have an external load balancer available, or where a cluster administrator wants to use their own load balancing solution.</p>
<p>We can install it via helm, <strong>please do this only on your main control node</strong>.</p>
<pre># First add metallb repository to your helm<br>helm repo add metallb <a href="https://metallb.github.io/metallb">https://metallb.github.io/metallb</a><br># Check if it was found<br>helm search repo metallb</pre>
<p>Example of "helm search repo metallb"</p>
<pre>root@cube01:~# helm search repo metallb<br>NAME CHART VERSION APP VERSION DESCRIPTION <br>metallb/metallb 0.13.7 v0.13.7 A network load-balancer implementation for Kube...</pre>
<p>Install MetalLB</p>
<pre>helm upgrade --install metallb metallb/metallb --create-namespace \<br>--namespace metallb-system --wait</pre>
<p>This will return output similar to the following:</p>
<pre>root@cube01:~# helm upgrade --install metallb metallb/metallb --create-namespace \<br>--namespace metallb-system --wait<br>Release "metallb" does not exist. Installing it now.<br>NAME: metallb<br>LAST DEPLOYED: Tue Jan 31 14:28:54 2023<br>NAMESPACE: metallb-system<br>STATUS: deployed<br>REVISION: 1<br>TEST SUITE: None<br>NOTES:<br>MetalLB is now running in the cluster.<br><br>Now you can configure it via its CRs. Please refer to the metallb official docs<br>on how to use the CRs</pre>
<p>We have now MetalLB installed, but it does not perform it function yet. We need to give it an IP range that it will be able to use. In our case, we will allow the MetalLB to use range 10.0.0.70 to .80, so we have 10 IPs to be assigned to our services.</p>
<pre>cat &lt;&lt; 'EOF' | kubectl apply -f -<br>apiVersion: metallb.io/v1beta1<br>kind: IPAddressPool<br>metadata:<br>  name: default-pool<br>  namespace: metallb-system<br>spec:<br>  addresses:<br>  - 10.0.0.70-10.0.0.80<br>---<br>apiVersion: metallb.io/v1beta1<br>kind: L2Advertisement<br>metadata:<br>  name: default<br>  namespace: metallb-system<br>spec:<br>  ipAddressPools:<br>  - default-pool<br>EOF</pre>
<p> </p>
<p>This will return output:</p>
<pre>ipaddresspool.metallb.io/default-pool created<br>l2advertisement.metallb.io/default created</pre>
<h3>
<br>Check</h3>
<p>To check if everything worked ok, there is a <a href="https://doc.traefik.io/traefik/providers/kubernetes-ingress/" target="_blank" rel="noopener">Traefik</a> installed by default, and this should get the first free IP address.</p>
<pre>root@cube01:~# kubectl get svc -n kube-system<br>NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE<br>kube-dns ClusterIP 10.43.0.10 &lt;none&gt; 53/UDP,53/TCP,9153/TCP 3h45m<br>metrics-server ClusterIP 10.43.201.157 &lt;none&gt; 443/TCP 3h45m<br>traefik LoadBalancer 10.43.99.73 10.0.0.70 80:31771/TCP,443:30673/TCP 3h44m<br><br>root@cube01:~# kubectl get events -n kube-system --field-selector involvedObject.name=traefik<br>LAST SEEN TYPE REASON OBJECT MESSAGE<br>61s Normal IPAllocated service/traefik Assigned IP ["10.0.0.70"]<br>60s Normal nodeAssigned service/traefik announcing from node "cube02" with protocol "layer2"</pre>
<p>As you can see from the first command we listed our services, and we can see that traefik is available on 10.0.0.70. The second command will filter out events for traefik, and we see it got the IP assigned.</p>
<p> </p>
<p><span class="wysiwyg-color-green120"><strong>Now we can assign individual IP from our network to services. How to do it will be shown in the sample deployment.</strong> </span></p>
<p> </p>
<h1><span class="wysiwyg-color-black">Traefik</span></h1>
<p>Traefik is a popular open-source reverse proxy and load balancer for microservices in a Kubernetes environment. It dynamically routes incoming requests to appropriate microservices based on their domain name, path, and other attributes. Traefik integrates with Kubernetes and other cloud-native tools to provide features such as service discovery, automatic SSL certificate management, and request routing based on custom rules. It also provides advanced features like circuit breaking, canary deployments, and traffic shaping.</p>
<p>Traefik is a pre-installed component in the Kubernetes cluster, if you followed the K3s installation method from our guide. To use Traefik effectively, it requires a working DNS server, which is external to the Kubernetes cluster. However, for local testing purposes, you can leverage the /etc/hosts file on your local machine and basically fake the DNS server. </p>
<div class="flex flex-grow flex-col gap-3">
<div class="min-h-[20px] flex flex-col items-start gap-4 whitespace-pre-wrap">
<div class="markdown prose w-full break-words dark:prose-invert light">
<p>The host file is located at:</p>
<ul>
<li>Mac: /private/etc/hosts</li>
<li>Windows: c:\windows\system32\drivers\etc\hosts</li>
<li>Linux: /etc/hosts</li>
</ul>
<p>You could edit this file and add entry like:</p>
<pre>10.0.0.70 turing-cluster turing-cluster.local</pre>
<p>And when you then enter <strong>https://turing-cluster.local</strong> in your browser, you should be redirected to a 404 page of Traefik. Mind you, this will work only on computers where you edit your host file. To make it global for your network, you need DNS server and all PCs to know about the DNS server.</p>
<p><span class="wysiwyg-color-green120"><strong>Now you can use Traefik to access your service under turing-cluster.local/service or service.turing-cluster.local (Of course you need to set up traefik to route to specific service)</strong></span></p>
</div>
</div>
</div>