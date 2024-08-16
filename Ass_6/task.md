Deploy a multi-container application in Kubernetes by creating a Pod that initially runs only an Nginx container serving a default page. Begin by verifying the basic deployment.✅

Next, introduce an init container that fetches a custom index.html file from a remote server using curl, and store this file in an emptyDir volume shared with the Nginx container.✅

The Nginx container should then serve this custom page.

Afterward, implement a liveness probe to monitor the health of the Nginx container by sending an HTTP GET request to the /healthz endpoint.✅

Finally, add a hostPath volume to store Nginx server logs, ensuring data persistence across Pod restarts.✅

The task should be completed progressively, with each step building on the previous one, allowing you to deploy, troubleshoot, and verify each component while deepening your understanding of Kubernetes concepts such as init containers, liveness probes, and volumes. Screenshot every step until completion

1. first create a nginx container

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: deploynginxserver
spec:
  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
```

**Explanation for this YAML file:**

I created an Nginx container that listens on port 80.

2. create a init container

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: deploynginxserver
spec:
  initContainers:
    - name: init-nginx
      image: curlimages/curl:latest
      command: ["curl"]
      args:
        ["-o", "/home/zoombie/data/index.html", "http://localhost/index.html"]
      volumeMounts:
        - name: html-volume
          mountPath: home/zoombie/data
  volumes:
    - name: html-volume
      emptyDir: {}
```

**Explanation for this YAML file:**

I used curl as the command in the init container, which first pulls the curlimages/curl image. The command then runs curl to fetch the content from http://localhost/index.html and saves it to the path /usr/share/nginx/html/index.html. This path is where I mounted the html-volume, a volume I created under the volumes section with the name html-volume.

3. Share the html-volume (of type emptyDir) between the init container and the Nginx container.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: deploynginxserver
spec:
  initContainers:
    - name: init-nginx
      image: curlimages/curl:latest
      command: ["curl"]
      args:
        ["-o", "/home/zoombie/data/index.html", "http://localhost/index.html"]
      volumeMounts:
        - name: html-volume
          mountPath: /home/zoombie/data

  containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - name: html-volume
          mountPath: /home/zoombie/data

  volumes:
    - name: html-volume
      emptyDir: {}
```

**Explanation for this YAML file:**

To share data between the init container and the Nginx container, I used a volume named html-volume of type emptyDir and mounted it to the same path in both containers.

4. Create a hostPath volume

```yaml
volumes:
  - name: host-path-volume
    hostPath:
      path: /home/zoombie/data
      type: Directory
```

5. To save the logs of the Nginx server.

```yaml
- name: host-path-volume
  mountPath: /var/log/nginx
```

**Explanation for this YAML file:**

In the Nginx container, mount the host path to /var/log/nginx, which is the directory where Nginx stores its logs

6. Create a liveness probe for the Nginx container using an HTTP GET request to the /healthz endpoint.

```yaml
containers:
    - name: nginx
      image: nginx:latest
      ports:
        - containerPort: 80
    livenessProbe: # A
      httpGet: # A
        path: /healthz # A
        port: 80 # A
```
