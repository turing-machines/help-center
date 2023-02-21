<p dir="auto">If you mess anything up in your Kubernetes cluster, and want to start from scratch, you can use Ansible's playbook<code>reset</code></p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>ansible-playbook reset.yml -i inventory/hosts.ini
</code></pre>
</div>
<h3 dir="auto">
<a id="user-content-shutting-down-the-turing-pi-cluster" class="anchor" href="https://github.com/turing-machines/v1-docs/blob/master/install-kubernetes-k3s/installing-k3s/reset-k3s.md#shutting-down-the-turing-pi-cluster" aria-hidden="true"></a>Shutting down the Turing Pi Cluster</h3>
<p dir="auto">Instead of just unplugging the Turing Pi, it's best to safely shut down all the Raspberry Pis. Instead of logging into each Pi and running the<code>shutdown</code>command, you can use Ansible, since it already knows how to connect to all the Pis. Just run this command:</p>
<div class="snippet-clipboard-content notranslate position-relative overflow-auto">
<pre class="notranslate" lang="text"><code>ansible all -i inventory/hosts.ini -a "shutdown now" -b
</code></pre>
</div>
<p dir="auto">Ansible will report failure for each server since the server shuts down and Ansible loses the connection, but you should see all the Pis power off after a minute or so. Now it's safe to unplug the Turing Pi.</p>
<p dir="auto"><em>This guide is based on the article</em><a href="https://www.jeffgeerling.com/blog/2020/installing-k3s-kubernetes-on-turing-pi-raspberry-pi-cluster-episode-3" rel="nofollow"><em>Installing K3s Kubernetes on the Turing Pi </em></a><em>by Jeff Geerling</em></p>