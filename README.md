# Deployment

**CI/CD Workflow (.github)**
```
# frontend-ci.yml
name: Frontend CI

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3 
      - run: npm ci 
      - run: npm run build
      - run: npm run test
      - run: npm run lint
```

**Build Pipeline**
```
1. Code Commit → Git Push
2. GitHub Actions Trigger
3. Install Dependencies (npm ci / mvn install)
4. Run Tests (Unit + Integration)
5. Build Artifacts
6. Security Scan (SonarQube)
7. Docker Image Build
8. Push to Container Registry
9. Deploy to Environment
```

Here’s a practical, step-by-step guide deploying a feature to production using Docker, Kubernetes, Jenkins, Prometheus, and Grafana:

- **Python**: https://github.com/kukuu/deployment/blob/main/python-deployment.md
- **Node** (a comprehensive deployment) - https://github.com/kukuu/deployment/blob/main/node-comprehensive-deployment.md
- **Kubernetes**: https://github.com/kukuu/kubernetes 
 
 
 
