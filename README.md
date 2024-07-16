# Node.js Application Deployment, Configuration, and Monitoring

## Overview

This project demonstrates how to configure, provision, and monitor a Node.js application using Docker, Kubernetes, Prometheus, and Grafana. The application is a simple RESTful API built with Node.js and Express.js.

## Directory Structure

nodejs-app/
├── config/
│ ├── development.json
│ ├── production.json
│ └── staging.json
├── src/
│ ├── app.js
│ ├── routes/
│ │ └── users.js
│ └── middleware/
│ ├── auth.js
│ └── errorHandler.js
├── Dockerfile
├── k8s/
│ ├── deployment.yaml
│ ├── service.yaml
│ └── ingress.yaml
├── prometheus.yml
├── Grafana_Dashboard.json
├── README.md
└── package.json

## Node.js Application Setup

1. **Navigate to the `src/` directory and run:**
   ```bash
   node app.js
   ```
   The application listens on port `3000` (or as configured in the environment).

## Docker Setup

1. **Build Docker image:**

   ```bash
   docker build -t nodejs-app .
   ```

2. **Run Docker container:**

   ```bash
   docker run -p 3000:3000 -e NODE_ENV=development nodejs-app
   ```

   The application should now be accessible at `http://localhost:3000`.

## Kubernetes Setup

1. **Start Minikube:**

   ```bash
   minikube start
   ```

2. **Apply Kubernetes configurations:**

   ```bash
   kubectl apply -f k8s/deployment.yaml
   kubectl apply -f k8s/service.yaml
   kubectl apply -f k8s/ingress.yaml
   ```

3. **Check the status of the deployment:**

   ```bash
   kubectl get pods
   kubectl get services
   kubectl get ingress
   ```

4. **Access the application:**
   Update your `/etc/hosts` file to include:
   ```plaintext
   127.0.0.1 nodejs-app.local
   ```

Then visit http://nodejs-app.local in your browser. If using Minikube, you might need to access it via Minikube’s IP:

$minikube ip

Monitoring Setup
Install Prometheus:

bash

helm install prometheus prometheus-community/prometheus --namespace monitoring --create-namespace
Install Grafana:

bash

helm install grafana grafana/grafana --namespace monitoring
Configure Prometheus:
Apply the prometheus.yml configuration file:

bash

kubectl apply -f prometheus.yml
Configure Alerting Rules:
Apply the alerting-rules.yml configuration file:

bash

kubectl apply -f alerting-rules.yml
Import Grafana Dashboard:

Access Grafana at http://<grafana-service-ip>:3000 (you can find the IP using kubectl get services -n monitoring).
Use the default username admin and the password from the Helm release output.
Import the dashboard JSON:
Go to Dashboards -> Manage -> Import.
Upload Grafana_Dashboard.json.
Testing and Verification
Unit Testing:

Use a testing framework like Mocha or Jest.
Example command for running tests (if tests are included):
bash

npm test
Integration Testing:

Verify the application works within the Minikube cluster:
bash

kubectl port-forward svc/nodejs-app-service 3000:80
Access the application at http://localhost:3000 and test API endpoints.
End-to-End Testing:

Test the complete deployment pipeline, from code changes to successful deployment and functionality in Minikube.
Monitoring Validation:

Verify that Prometheus is collecting metrics:
Access Prometheus UI at http://<prometheus-service-ip>:9090.
Ensure Grafana displays metrics:
Access Grafana UI and check the dashboard created.
Troubleshooting
Docker Issues:

Check Docker container logs:
bash

docker logs <container-id>
Verify Dockerfile and application code configurations.
Kubernetes Issues:

Check Kubernetes pod logs:
bash

kubectl logs <pod-name>
Verify deployments and services configurations:
bash

kubectl describe deployment <deployment-name>
kubectl describe service <service-name>
Monitoring Issues:

Ensure Prometheus is scraping metrics correctly:
Access Prometheus UI at http://<prometheus-service-ip>:9090 and check scrape targets.
Verify Grafana is connected to Prometheus and data sources are configured correctly.
Conclusion
This project provides a comprehensive setup for deploying, configuring, and monitoring a Node.js application using Docker, Kubernetes, Prometheus, and Grafana. It covers essential steps and best practices to ensure a smooth deployment process and effective monitoring setup. For additional features or improvements, consider implementing advanced deployment strategies or custom metrics.
