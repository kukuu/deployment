# Node Deployment

Below is an end-to-end example of developing, testing, deploying, and monitoring a React application using Node.js/TypeScript for the backend, with Docker, Kubernetes, AWS, and monitoring via Prometheus, ELK Stack, and Grafana. 

This work includes unit tests, integration tests, UAT, smoke tests, and deployment steps. It covers development, testing, deployment, and monitoring for a React app with a Node.js backend. 

**Step 1: Set Up the React App and Node.js Backend**

1. Create React App:

```
npx create-react-app my-app --template typescript
cd my-app
```

2. Set Up Node.js Backend:

i. Create a backend folder inside the project:

```
mkdir backend
cd backend
npm init -y
npm install express typescript ts-node @types/express
```

ii. Add a tsconfig.json for TypeScript:

```
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "./dist"
  }
}
```

iii. Create a simple Express server (backend/src/index.ts): 

```
import express from 'express';
const app = express();
const port = 3001;

app.get('/api', (req, res) => {
  res.json({ message: 'Hello from the backend!' });
});

app.listen(port, () => {
  console.log(`Backend running on http://localhost:${port}`);
});

```

3. Run the Backend in Development Mode:

```
npx ts-node src/index.ts
```

4. Run the React App in Development Mode:

```
cd ..
npm start
```

**- Step 2: Write Unit and Integration Tests**

1. Unit Tests for Backend (Jest):

i. Install Jest:

```
npm install --save-dev jest ts-jest @types/jest
```

ii. Add a test file (backend/src/index.test.ts):

```
import request from 'supertest';
import app from './index';

describe('GET /api', () => {
  it('responds with JSON', async () => {
    const response = await request(app).get('/api');
    expect(response.status).toBe(200);
    expect(response.body).toEqual({ message: 'Hello from the backend!' });
  });
});
```

iii. Run tests:

```
npx jest
```


2. Integration Tests for Frontend (React Testing Library):

i. Install React Testing Library:

```
npm install --save-dev @testing-library/react @testing-library/jest-dom
```

ii. Add a test file (src/App.test.tsx):

```
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});

```

iii. Run tests:

```
npm test
```


**- Step 3: Dockerize the Application**

1. Create a Dockerfile for the Backend:

```
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
CMD ["node", "dist/index.js"]
```

2. Create a Dockerfile for the Frontend:

```
FROM node:16 as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

3. Build and Run Docker Containers:

```
docker build -t my-app-backend -f backend/Dockerfile .
docker build -t my-app-frontend -f Dockerfile .
docker run -p 3001:3001 my-app-backend
docker run -p 80:80 my-app-frontend
```

**- Step 4: Kubernetes Deployment**

1. Create Kubernetes Manifests:

i. k8s/backend-deployment.yaml:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: backend
        image: my-app-backend
        ports:
        - containerPort: 3001
```

ii. k8s/frontend-deployment.yaml: 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: frontend
        image: my-app-frontend
        ports:
        - containerPort: 80
```

2. Deploy to Kubernetes:

```
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/frontend-deployment.yaml

```

**- Step 5: AWS Migration**

1. Push Docker Images to AWS ECR:

```
aws ecr create-repository --repository-name my-app-backend
docker tag my-app-backend:latest <aws-account-id>.dkr.ecr.<region>.amazonaws.com/my-app-backend:latest
docker push <aws-account-id>.dkr.ecr.<region>.amazonaws.com/my-app-backend:latest

```

2. Deploy to AWS EKS:

i. Update Kubernetes manifests to use ECR images.

ii. Apply manifests to EKS: 

```
kubectl apply -f k8s/backend-deployment.yaml
kubectl apply -f k8s/frontend-deployment.yaml
```

**- Step 6: Monitoring and Logging**

1. Prometheus and Grafana:

i. Deploy Prometheus and Grafana using Helm:

```
helm install prometheus prometheus-community/prometheus
helm install grafana grafana/grafana

```

ii. Configure Prometheus to scrape metrics from the backend.

2. ELK Stack for Logging:

i. Deploy Elasticsearch, Logstash, and Kibana:

```
helm install elasticsearch elastic/elasticsearch
helm install kibana elastic/kibana
```

ii. Send logs from the backend to Elasticsearch.

3. Visualize Metrics in Grafana:

Create dashboards in Grafana to monitor application performance.
