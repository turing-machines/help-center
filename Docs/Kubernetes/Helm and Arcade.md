<h1>Helm</h1>
<p>Helm is a package manager for Kubernetes that helps you manage and distribute applications in a more organized and easy manner. It provides a way to package, configure and deploy your applications in a repeatable and version-controlled manner. With Helm, you can manage and share your application templates, making it easier to roll out updates and rollbacks, as well as share your applications with others.</p>
<p>We will follow the official install process described <strong><a href="https://helm.sh/docs/intro/install/" target="_blank" rel="noopener noreferrer">HERE</a></strong>.</p>
<p>More information about what Helm is and does can be found <a href="https://helm.sh/" target="_blank" rel="noopener noreferrer"><strong>HERE</strong></a>.</p>
<p><strong>Do the following on your Master node only.</strong></p>
<pre>#Make sure GIT is installed<br>apt -y install git<br>#We need to fix kubeconfig file for helm to stop complaining<br>export KUBECONFIG=~/.kube/config<br>mkdir ~/.kube 2&gt; /dev/null<br>sudo k3s kubectl config view --raw &gt; "$KUBECONFIG"<br>chmod 600 "$KUBECONFIG"<br>echo "KUBECONFIG=$KUBECONFIG" &gt;&gt; /etc/environment<br>#Switch to home directory<br>cd<br>#Create a directory for helm<br>mkdir helm<br>#Switch to helm directory<br>cd helm<br>#Download helm installer<br>curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3<br>#change permissions to execute<br>chmod 700 get_helm.sh<br>#install helm<br>./get_helm.sh<br>#check if helm is installed<br>root@cube01:~/helm# helm version<br>version.BuildInfo{Version:"v3.11.0", GitCommit:"472c5736ab01133de504a826bd9ee12cbe4e7904", GitTreeState:"clean", GoVersion:"go1.18.10"}</pre>
<h1>Arkade</h1>
<p>Arkade is a simple, one-liner tool for installing Kubernetes applications on any cluster. It automates the installation process and helps to make it easier and faster to get up and running with your desired applications in a Kubernetes cluster. With Arkade, you don't have to worry about manual installation, configuration or dependency management as it takes care of everything for you.</p>
<p>We will follow the official install process described <a href="https://github.com/alexellis/arkade#getting-arkade" target="_blank" rel="noopener noreferrer"><strong>HERE</strong></a>.</p>
<p>More information about what Arkade is and does can be found <a href="https://github.com/alexellis/arkade#usage-overview" target="_blank" rel="noopener noreferrer"><strong>HERE</strong></a>.</p>
<p><strong>Do the following on your Master node only.</strong></p>
<pre>#Execute<br>curl -sLS https://get.arkade.dev | sudo sh<br><br>#Check version:<br><br>root@cube01:~/helm# arkade version
            _             _      
  __ _ _ __| | ____ _  __| | ___ 
 / _` | '__| |/ / _` |/ _` |/ _ \
| (_| | |  |   &lt; (_| | (_| |  __/
 \__,_|_|  |_|\_\__,_|\__,_|\___|

Open Source Marketplace For Developer Tools

Version: 0.8.60
Git Commit: 9c7df2b619a90f8e609bc959495bcdc65c3b9455

 🐳 arkade needs your support: https://github.com/sponsors/alexellis</pre>