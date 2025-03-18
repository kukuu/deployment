# Python Deployment

- Step 1: Local Development and Unit Testing

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

- Step 2: Push Code to Version Control
