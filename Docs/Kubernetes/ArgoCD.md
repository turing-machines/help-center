<p><strong><a href="https://argoproj.github.io/cd/" target="_blank" rel="noopener noreferrer">ArgoCD</a> </strong>is a GitOps tool for Kubernetes. It provides a way to manage applications in a Git repository and continuously deploy them to a cluster. Argo CD helps to ensure that the desired state of applications in the cluster matches the state specified in the Git repository.</p>
<p>The benefits of using Argo CD in a Kubernetes environment are:</p>
<ul>
<li>
<strong>Version control</strong>: ArgoCD integrates with Git, allowing for version control of your application configurations.</li>
<li>
<strong>Automated deployment</strong>: ArgoCD continuously monitors the Git repository and automatically deploys changes to the cluster.</li>
<li>
<strong>Consistency:</strong> ArgoCD ensures that the state of the cluster and the Git repository are in sync, preventing drift and ensuring consistency.</li>
<li>
<strong>Easy rollback</strong>: ArgoCD makes it easy to roll back to previous versions of an application in the Git repository.</li>
<li>
<strong>Auditability:</strong> ArgoCD provides an audit trail of changes to applications, making it easier to understand and track changes over time.</li>
</ul>
<p>Overall, Argo CD can help simplify and automate application deployment and management in a Kubernetes environment.</p>
<h1>Install</h1>
<p>Installation is very straight forward, ArgoCD do not require persistent storage and does support Arm64 architecture out of the box.</p>
<p>On your primary control node, do:</p>
<pre>#create namespace<br>root@cube01:~# kubectl create namespace argocd<br>#Install as on any other cluster<br>root@cube01:~# kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml</pre>
<p>Please wait for the ArgoCD to finish deployment, you can see it done when all pods are in "Running" State.</p>
<pre>root@cube01:~# kubectl get pods -n argocd<br>NAME                                             READY STATUS  RESTARTS   AGE<br>argocd-notifications-controller-7c946895bb-nb2qm  1/1 Running 1 (13h ago) 15h<br>argocd-redis-598f75bc69-cv25j                     1/1 Running 1 (13h ago) 15h<br>argocd-applicationset-controller-7c86dd8cd7-mnw59 1/1 Running 1 (13h ago) 15h<br>argocd-dex-server-786fb4b8b-pc8mz                 1/1 Running 1 (13h ago) 15h<br>argocd-repo-server-648db4756c-cw6c4               1/1 Running 1 (13h ago) 15h<br>argocd-application-controller-0                   1/1 Running 1 (13h ago) 15h<br>argocd-server-6cfb678659-lwm7v                    1/1 Running 1 (13h ago) 15h<br><br></pre>
<p>ArgoCD will automatically generate a <strong>root password</strong> that you can use to log in. To get it, execute the following command:</p>
<pre>kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo</pre>
<h2>UI</h2>
<p>You can access the GUI of ArgoCD multiple ways.</p>
<h3>
<strong>Option 1</strong>: Port forwarding the service to your localhost.</h3>
<pre>kubectl port-forward svc/argocd-server -n argocd 8080:443</pre>
<p>You should be able to access it on <a href="https://localhost:8080">https://localhost:8080</a></p>
<h3>
<strong>Option 2</strong>: (Preferred) Use MetalLB to assign Unique IP to ArgoCD UI</h3>
<pre>kubectl patch service argocd-server -n argocd --patch '{ "spec": { "type": "LoadBalancer", "loadBalancerIP": "10.0.0.72" } }'</pre>
<p>Change the IP to one free from range defined for MetalLB.</p>
<h3>
<strong>Option 3</strong>: Use build in Traefik to route base on DNS</h3>
<p>Follow the official guide <a href="https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#traefik-v22" target="_blank" rel="noopener noreferrer"><strong>HERE</strong></a>. This however require also to do some changes to configuration of ArgoCD, like disabling HTTPS internally and having Traefik terminate HTTPS connections instead.</p>
<h2>Deploying simple application</h2>
<p>Let's make sample deployments that can be actually also useful, our "go to" test deployment is <a href="https://github.com/tarampampam/error-pages" target="_blank" rel="noopener noreferrer">error-pages</a></p>
<p>We have prepared a <a href="https://gitlab.com/vstrycek/turingpi-error-pages" target="_blank" rel="noopener noreferrer"><strong>git repository</strong></a> with Kubernetes deployment. And what it does, is deploy error-pages container and redirect all 404 and other errors in Traefik to it. Showing much more pleasing error page.</p>
<p><strong><span class="wysiwyg-color-red">We highly recommend cloning this repository to your own.</span></strong></p>
<p><strong>Open ArgoCD UI</strong> and log in with admin/&lt;password&gt; and click on <strong>"Create Application"</strong></p>
<p><strong><img src="https://help.turingpi.com/hc/article_attachments/9084300029085" alt="argocd_1.png"></strong></p>
<p> </p>
<p><strong>Fill in the following:</strong></p>
<ul>
<li>
<strong>Application Name</strong>: error-pages</li>
<li>
<strong>Project Name</strong>: default</li>
<li>
<strong>Sync Policy</strong>: Manual</li>
<li>
<strong>Auto-Create Namespace:</strong> "check"</li>
<li>
<strong>Repository URL(GIT):</strong> <a href="https://gitlab.com/vstrycek/turingpi-error-pages.git">https://gitlab.com/vstrycek/turingpi-error-pages.git</a>
</li>
<li>
<strong>Revision</strong>: HEAD</li>
<li>
<strong>Path</strong>: .</li>
<li>
<strong>Cluster URL</strong>: <a href="https://kubernetes.default.svc">https://kubernetes.default.svc</a>
</li>
<li>
<strong>Namespace</strong>: error-pages</li>
</ul>
<p> </p>
<div class="columns small-10">
<div>
<div class="select argo-field argo-has-value" style="display: inline-block;"> </div>
</div>
<div>
<div class="columns small-10">
<div>
<div class="argo-form-row">
<div><img src="https://help.turingpi.com/hc/article_attachments/9084496939677" alt="argocd_general.png"></div>
<div> </div>
</div>
</div>
</div>
</div>
</div>
<p><img src="https://help.turingpi.com/hc/article_attachments/9084510965661" alt="argocd_src.png"></p>
<p>Then hit <strong>"Create" </strong>on top of the page. Your app should appear in ArgoCD main page, but since we have chosen "Manual" for sync, it will not automatically deploy. Y<span class="wysiwyg-color-red">ou should not in any case have automatic deployment from Git that is not under your control !</span></p>
<p><span class="wysiwyg-color-black">Now you can hit "<strong>Sync"</strong> and then <strong>"Synchronize</strong>" on the panel that appear. Watch how the app deploys by clicking on the app panel.<br></span></p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9084511061533" alt="argocd_sync.png"></p>
<p>If everything went ok, you should see something like this:</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9084846247709" alt="argocd_deploy.png"></p>
<p>In our guide, we set up MetalLB and Traefik got IP 10.0.0.70, then we added <code>turing-cluster.local</code> into our host file. So when we go to http or https://<a href="http://turing-cluster.local">http://turing-cluster.local</a> we should get a nice looking error page.</p>
<p><img src="https://help.turingpi.com/hc/article_attachments/9085108088989" alt="error_page_ok.png"></p>
<h2>Enable CLI from GUI</h2>
<p>ArgoCD allows CLI access directly to your containers, but this feature is not turned on by default. To turn it on, do the following:</p>
<p><strong>1. Patch role argocd-server </strong></p>
<pre>kubectl patch role argocd-server -n argocd --type=json -p='[{"op": "add", "path": "/rules/-", "value": {"apiGroups": [""], "resources": ["pods/exec"], "verbs": ["create"]}}]'</pre>
<p><strong>2. Patch ConfigMap argocd-cm</strong></p>
<pre>kubectl patch configmap argocd-cm -n argocd -p '{"data": {"exec.enabled": "true"}}'</pre>
<p><strong>3. Delete all ArgoCD pods (restarting the service)</strong></p>
<pre>kubectl delete pods -n argocd --field-selector=status.phase=Running</pre>
<p>Wait for the pods to come back into "Running" state, checking with:</p>
<pre> kubectl get pod -n argocd</pre>
<p>As <strong>admin</strong> user, now you should have option to enter CLI for pods. (Error pages is not the best example since this deployment have some serious security restrictions)</p>
<p class="wysiwyg-text-align-center"><img src="https://help.turingpi.com/hc/article_attachments/9087350099741" alt="argocd_exec.png"></p>
<p class="wysiwyg-text-align-left">For creating users and adding them possibility to interact with CLI please check the ArgoCD <a href="https://argo-cd.readthedocs.io/en/stable/" target="_blank" rel="noopener noreferrer"><strong>documentation</strong></a>.</p>
<p> </p>