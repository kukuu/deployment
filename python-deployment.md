# Python Deployment

**- Step 1: Local Development and Unit Testing**

i. Write Code and Unit Tests:

i. Develop the feature and write unit tests using a framework like JUnit (Java), pytest (Python), or Mocha (Node.js).

Example unit test (Python):

```
def test_feature():
    assert my_feature() == expected_output

```

2. Containerize the Application with Docker:

i. Create a Dockerfile to build the application image.

Example Dockerfile:

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

i. Commit and Push Code:

ii. Push the feature branch to GitHub or GitLab.

Example:

```

git checkout -b feature-branch
git add .
git commit -m "Add new feature"
git push origin feature-branch

```
**- Step 3: CI/CD Pipeline with Jenkins**

1. Set Up Jenkins Pipeline:

i. Create a Jenkinsfile to define the CI/CD pipeline.

Example Jenkinsfile: 

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

Deploy the feature to a staging environment using Kubernetes.

Run integration and end-to-end tests using tools like Selenium or Cypress.
