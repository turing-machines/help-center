<h1>Installing K3s</h1>
<p>We will install K3s using ansible.</p>
<p>Download or clone the <code>k3s-ansible</code> repository, modify the playbook inventory, and run it:</p>
<ol>
<li>Download using the 'Download ZIP' link on GitHub on <a href="https://github.com/rancher/k3s-ansible">https://github.com/rancher/k3s-ansible</a>
</li>
<li>Edit the Ansible inventory file <code>inventory/hosts.ini</code>, and replace the examples with the IPs or <a href="https://docs.turingpi.com/install-kubernetes-k3s/change-hostnames">hostnames</a> of your master and nodes. This file describes the K3s masters and nodes to Ansible as it installs K3s.</li>
<li>Edit the <code>inventory/group_vars/all.yml</code> file and change the <code>ansible_user</code> to <code>pirate</code>.</li>
<li>Run <code>ansible-playbook site.yml -i inventory/hosts.ini</code> and wait.</li>
</ol>
<p>To connect to the cluster, once it's built, you need to grab the <code>kubectl</code> configuration from the master:</p>
<pre><code class="language-text">scp pirate@turing-master:~/.kube/config ~/.kube/config-turing-pi</code></pre>
<p>Make sure you have <code>kubectl</code> installed on your computer (you can install it following <a href="https://kubernetes.io/docs/tasks/tools/install-kubectl/">these directions</a>).</p>
<p>Then set the <code>KUBECONFIG</code> environment variable, and start running <code>kubectl</code> commands:</p>
<pre><code class="language-text">export KUBECONFIG=~/.kube/config-turing-pi
kubectl get nodes</code></pre>
<p>You should get a list of all the Pi servers; if you do, congratulations! Your cluster is up and running.</p>
<p><em>This guide is based on the article</em> <a href="https://www.jeffgeerling.com/blog/2020/installing-k3s-kubernetes-on-turing-pi-raspberry-pi-cluster-episode-3"><em>Installing K3s Kubernetes on the Turing Pi</em></a> <em>by Jeff Geerling</em></p>