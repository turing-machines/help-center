<p>We suggest utilizing ArgoCD for GitOps-based deployments to keep track about what is running and where more easily.</p>
<p>However, this sample deployment will covers the majority of the systems we have established, including MetalLB and Persistent Storage. We will create separate deployment YAML files and apply them sequentially without ArgoCD.</p>
<p>We plan to deploy a basic Redis server that will feature persistent storage, which will be linked to the pod as it moves throughout the cluster. The server will also have its own unique IP address (10.0.0.73) thanks to the use of MetalLB.</p>
<p><strong>Create a directory</strong> to store your configuration files.</p>
<pre>mkdir redis-deployment<br>cd redis-deployment</pre>
<p>We will organize all components related to Redis into its own namespace.</p>
<pre>kubectl create namespace redis-server</pre>
<h2>pvc.yaml</h2>
<p>Create our first configuration file: <strong>pvc.yaml. </strong>We are going to define persistent storage for our deployment. In this example, we are using Longhorn storage class, but you can easily rename it to<strong> nfs-client </strong>if you have installed NFS StorageClass.</p>
<p>To list your StorageClasses run following command:</p>
<pre>kubectl get storageclass</pre>
<p>Inside pvc.yaml</p>
<pre>apiVersion: v1<br>kind: PersistentVolumeClaim<br>metadata:<br>  name: redis-pvc<br>  namespace: redis-server<br>spec:<br>  accessModes:<br>    - ReadWriteOnce<br>  storageClassName: longhorn<br>  resources:<br>    requests:<br>      storage: 5Gi</pre>
<h2>deployment.yaml</h2>
<p>Here we will define what container to run, what storage to mount to it and where.</p>
<pre>apiVersion: apps/v1<br>kind: Deployment<br>metadata:<br>  name: redis-server<br>  namespace: redis-server<br>spec:<br>  replicas: 1<br>  selector:<br>    matchLabels:<br>      app: redis-server<br>  template:<br>    metadata:<br>      labels:<br>        app: redis-server<br>        name: redis-server<br>    spec:<br>      nodeSelector:<br>        node-type: worker<br>      containers:<br>      - name: redis-server<br>        image: redis<br>        args: ["--appendonly", "yes"]<br>        ports:<br>          - name: redis-server<br>            containerPort: 6379<br>        volumeMounts:<br>          - name: lv-storage<br>            mountPath: /data<br>        env:<br>          - name: ALLOW_EMPTY_PASSWORD<br>            value: "yes"<br>      volumes:<br>        - name: lv-storage<br>          persistentVolumeClaim:<br>            claimName: redis-pvc</pre>
<p><strong>Not some details like:</strong></p>
<ul>
<li>
<strong> nodeSelector</strong> - where near the start of this guide we have labeled our nodes, and this allows us to tell Kubernetes where this deployment can run.</li>
<li>
<strong>volumeMounts</strong> - Is mounting volume named lv-storage (defined from our PVC in the bottom of the config) in container to path /data</li>
</ul>
<h2>service.yaml</h2>
<p>In this file, we will define Kubernetes service, telling our cluster how to access the container from deployment.yaml</p>
<pre>apiVersion: v1<br>kind: Service<br>metadata:<br>  name: redis-server<br>  namespace: redis-server<br>spec:<br>  selector:<br>    app: redis-server<br>  type: LoadBalancer<br>  ports:<br>    - name: redis-port<br>      protocol: TCP<br>      port: 6379<br>      targetPort: 6379<br>      loadBalancerIP: 10.0.0.73</pre>
<p>Here is important to node <strong>selector </strong>which basically looks for container with app=redis-server, and we have defined that in deployment.yaml under <strong>template.</strong> Next important is the <strong>loadBalancerIP, </strong>here you can specify IP from MetalLB range.</p>
<h1>Deploy</h1>
<p>You can deploy all the files at once by just providing a folder for kubect deploy command, but we will do it in logical order for educational purposes.</p>
<pre>kubectl apply -f pvc.yaml</pre>
<p>Check if PVC was created</p>
<pre>root@cube01:~/redis-deployment# kubectl get pvc -n redis-server<br>NAME      STATUS   VOLUME                                   CAPACITY ACCESS MODES STORAGECLASS AGE<br>redis-pvc Bound    pvc-5cc8e80c-182a-4f50-b25a-d52904ec31d9 5Gi       RWO            longhorn    66s</pre>
<p>You can also get more details with:</p>
<pre>kubectl describe pvc redis-pvc -n redis-server</pre>
<p>Deploy the deployment.yaml</p>
<pre>kubectl apply -f deployment.yaml</pre>
<p>Check and wait until container is "Running"</p>
<pre>root@cube01:~/redis-deployment# kubectl get pods -n redis-server<br>NAME                        READY  STATUS  RESTARTS AGE<br>redis-server-8db957bc6-dpxtx 1/1   Running    0      74s</pre>
<p>Lastly, create the service.</p>
<pre>kubectl apply -f service.yaml</pre>
<p>Check the service:</p>
<pre>root@cube01:~/redis-deployment# kubectl get svc redis-server -n redis-server<br>NAME            TYPE        CLUSTER-IP  EXTERNAL-IP   PORT(S)       AGE<br>redis-server LoadBalancer 10.43.233.56    10.0.0.73 6379:31424/TCP 2m46s</pre>
<p><span class="wysiwyg-color-green120"><strong>Now we have Redis server on port 10.0.0.73, with persistent storage and external IP.</strong> </span></p>
<h1><span class="wysiwyg-color-black">Cleanup</span></h1>
<p><span class="wysiwyg-color-black">You can easily remove everything by using <strong>kubectl delete</strong> with the same files in reverse order.</span></p>
<pre>kubectl delete -f service.yaml<br>kubectl delete -f deployment.yaml <br>kubectl delete -f pvc.yaml </pre>