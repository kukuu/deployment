# Python Deployment

**- Step 1: Local Development and Unit Testing**

1.  Write Code and Unit Tests:

i. Develop the feature and write unit tests using a framework like JUnit (Java), pytest (Python), or Mocha (Node.js).

ii. Example unit test (Python):

```
def test_feature():
    assert my_feature() == expected_output

```

2. Containerize the Application with Docker:

i. Create a Dockerfile to build the application image.

i. Example Dockerfile:

```
FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]

```

ii. Build and test the Docker image locally:

```
docker build -t my-app:latest .
docker run -p 5000:5000 my-app:latest

```

**- Step 2: Push Code to Version Control** 

1. Commit and Push Code:

i. Push the feature branch to GitHub or GitLab.

ii. Example:

```

git checkout -b feature-branch
git add .
git commit -m "Add new feature"
git push origin feature-branch

```
**- Step 3: CI/CD Pipeline with Jenkins**

1. Set Up Jenkins Pipeline:

i. Create a Jenkinsfile to define the CI/CD pipeline.

ii. Example Jenkinsfile: 

```
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t my-app:${GIT_COMMIT} .'
            }
        }
        stage('Unit Tests') {
            steps {
                sh 'pytest tests/'
            }
        }
        stage('Push to Registry') {
            steps {
                sh 'docker tag my-app:${GIT_COMMIT} my-registry/my-app:${GIT_COMMIT}'
                sh 'docker push my-registry/my-app:${GIT_COMMIT}'
            }
        }
        stage('Deploy to Staging') {
            steps {
                sh 'kubectl apply -f k8s/staging-deployment.yaml'
            }
        }
    }
}
```

2. Run Integration and QA/UAT Tests:

i. Deploy the feature to a staging environment using Kubernetes.

ii. Run integration and end-to-end tests using tools like Selenium or Cypress.


**- Step 4: Deploy to Production**

1. Update Kubernetes Manifests:

i. Modify the production deployment YAML to use the new image tag.

ii. Example k8s/production-deployment.yaml:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: my-app
        image: my-registry/my-app:${GIT_COMMIT}
        ports:
        - containerPort: 5000
```
2. Deploy to Production:

i. Apply the updated Kubernetes manifest:

```
kubectl apply -f k8s/production-deployment.yaml
```

**- Step 5: Monitoring with Prometheus and Grafana**

1. Set Up Prometheus:

i. Deploy Prometheus in the Kubernetes cluster to scrape metrics.

ii. Example k8s/prometheus-config.yaml:

```
global:
  scrape_interval: 15s
scrape_configs:
- job_name: 'my-app'
  static_configs:
  - targets: ['my-app:5000']

```

2. Expose Metrics in the Application:

i. Use a library like Prometheus Client to expose metrics.

ii. Example (Python): 

```
from prometheus_client import start_http_server, Counter
REQUEST_COUNT = Counter('request_count', 'Total HTTP Requests')
@app.route('/')
def index():
    REQUEST_COUNT.inc()
    return "Hello, World!"
start_http_server(8000)

```

3. Visualize Metrics in Grafana:

i. Deploy Grafana and connect it to Prometheus.

ii. Create a dashboard to visualize metrics like request count, error rates, and response times. 

**- Step 6: Rollback (if needed)**

1. Rollback to Previous Version:

i. If issues arise, rollback using Kubernetes: 

```
kubectl rollout undo deployment/my-app

```

This end-to-end process ensures a robust, automated deployment pipeline with monitoring and visualization for production readiness.
