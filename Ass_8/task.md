Set up a Kubernetes environment to deploy a simple blog application with a frontend and backend,
focusing on managing application updates, storage, and autoscaling. Create a namespace called
blog and label all resources within this namespace with app=blog. The application consists of
two components:

1.  `Frontend`: Use the Nginx image nginx:alpine to serve static content for the blog.
    Deploy this in a Deployment named frontend-deployment with 2 replicas. Expose the
    frontend using a Service of type ClusterIP named frontend-service. Create an
    Ingress resource to expose the frontend externally at the / path. Configure Nginx to serve
    content from /usr/share/nginx/html, the default content directory.

2.  `Backend`: Deploy a simple web application using the official MySQL image mysql:5.7
    for the database backend. Deploy this in a StatefulSet named mysql-statefulset to
    ensure data persistence across pod restarts. The StatefulSet should mount a
    PersistentVolumeClaim (PVC) at /var/lib/mysql to store the MySQL database. Expose
    the MySQL service internally using a Service of type ClusterIP named
    mysql-service.

3.  `Storage`: Create a PersistentVolumeClaim (PVC) named mysql-pvc to persist the MySQL
    database used by the backend. This PVC should be mounted at the path /var/lib/mysql
    in the MySQL pods.

4.  `Horizontal Pod Autoscaler (HPA)`: Implement an HPA for the frontend Deployment to automatically scale the number of replicas based on CPU utilization. Set the target CPU
    utilization to 50%, allowing the HPA to scale the frontend pods between 2 and 5 replicas
    based on demand.
5.  `Configuration and Secrets`: Store the MySQL root password in a Secret named
    mysql-secret, and configure the MySQL database name and user credentials in a
    ConfigMap named mysql-config.

**Verification:**

- Test the deployment by accessing the blog frontend through the Ingress.

- confirming that the backend MySQL database is functioning correctly.

- Simulate increased traffic to trigger the HPA, and verify that the frontend scales appropriately.

- Also, simulate an update to the frontend by changing the image version (e.g., nginx:alpine to nginx:latest) and ensure the update rolls out smoothly without downtime.

- Ensure all resources are properly labeled.

- PVCis correctly attached to the backend, preserving data across pod restarts
