"# My DevOps Project" 
------
1)Perform Git operations to commit, push, merge branches, and create a pull request on GitHub.

Step 1 – Initialise repo & first commit

Initialise a local repository

git init myproject
cd myproject

Create a file and make the first commit

echo "# My DevOps Project" > README.md
git add README.md
git commit -m "Initial commit: add README"

Step 2 – Create a feature branch & make changes

git checkout -b feature/hello

echo "Hello from feature branch" > hello.txt
git add hello.txt
git commit -m "feat: add hello.txt"

Step 3 – Push to GitHub & open Pull Request

git remote add origin https://github.com/<username>/myproject.git
git push -u origin feature/hello

On GitHub → go to your repo → click Compare & pull request → add a description → click Create pull request.
Step 4 – Merge & push main

git checkout main
git merge feature/hello
git push origin main
git log --oneline --graph



------
2)Containerise a Flask application using Docker and deploy the image to Docker Hub.

app.py

from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Dockerised Flask!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
    
requirements.txt
text
copy
flask

Dockerfile

FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]


docker build -t <dockerhub-username>/flask-app:latest .

Test locally → visit http://localhost:5000

docker run -d -p 5000:5000 <dockerhub-username>/flask-app:latest

Login and push

docker login
docker push <dockerhub-username>/flask-app:latest

------
3) Develop and demonstrate a simple monolithic application model using Python or Flask

from flask import Flask, request, jsonify

app = Flask(__name__)
students = {}
next_id = 1

@app.route('/students', methods=['GET'])
def get_students():
    return jsonify(list(students.values()))

@app.route('/students', methods=['POST'])
def add_student():
    global next_id
    data = request.get_json()
    student = {'id': next_id, 'name': data['name'], 'roll': data['roll']}
    students[next_id] = student
    next_id += 1
    return jsonify(student), 201

@app.route('/students/<int:sid>', methods=['DELETE'])
def delete_student(sid):
    students.pop(sid, None)
    return jsonify({'msg': 'Deleted'})

if __name__ == '__main__':
    app.run(debug=True)


pip install flask
python app.py

Add a student

curl -X POST http://localhost:5000/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","roll":"CS001"}'

List all students

curl http://localhost:5000/students



------
4) Create a CI pipeline using GitHub Actions to build and push a Docker image of a Python app to Docker Hub.

Step 1 
Repo → Settings → Secrets and variables → Actions → New repository secret:
DOCKER_USERNAME — your Docker Hub username
DOCKER_PASSWORD — your Docker Hub access token

.github/workflows/docker-publish.yml

name: Build and Push Docker Image

on:
  push:
    branches: [ main ]

jobs:
  build-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker image
        run: docker build -t ${{ secrets.DOCKER_USERNAME }}/python-app:latest .

      - name: Push to Docker Hub
        run: docker push ${{ secrets.DOCKER_USERNAME }}/python-app:latest

        
-----
5)Configure a Jenkins pipeline to automate the building and deployment of a Dockerized application from GitHub.

jenkins cmd:

docker volume create jenkins-data

docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins-data:/var/jenkins_home -v //var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts

docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword


docker exec -u root jenkins bash -c "apt-get update && apt-get install -y docker.io"


permission error :
docker exec -u root jenkins chmod 666 /var/run/docker.sock

-----
6)
kubernetes

**Step 1 – Create a deployment**

kubectl create deployment myapp --image=nginx:latest
kubectl get deployments
kubectl get pods

**Step 2 – Scale the deployment**

kubectl scale deployment myapp --replicas=3
kubectl get pods

kubectl scale deployment myapp --replicas=1

**Step 3 – Expose, inspect & clean up**

kubectl expose deployment myapp --port=80 --type=NodePort
kubectl get services
kubectl describe pod <pod-name>
kubectl logs <pod-name>

Clean up

kubectl delete deployment myapp
kubectl delete service myapp

**Step 4 – Using a YAML manifest (deployment.yaml)**

kubectl apply -f deployment.yaml
kubectl get all

---

7) Create a GitHub Actions pipeline to automatically build and push a Docker image of a Python app to Docker Hub.

.github/workflows/build-push.yml

name: Docker Build & Push

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  docker:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ secrets.DOCKER_USERNAME }}/python-app:latest


----
8) Containerise a Flask application using Docker 8

app.py

from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return "<h1>Flask app running in Docker!</h1>"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
    
Dockerfile

FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]

Build & run

docker build -t flask-container .
docker run -d -p 5000:5000 --name myflask flask-container
docker ps
# Visit http://localhost:5000

-----
9)Clone a GitHub repository, modify a file and push the changes back to the repository.

Step 1 – Clone

git clone https://github.com/<username>/<repo-name>.git
cd <repo-name>

Step 2 – Modify a file

echo "Updated content" >> README.md
cat README.md
git diff

Step 3 – Commit & push

git add README.md
git commit -m "docs: update README with new content"
git push origin main
git log --oneline -3

----

10) Configure a GitHub Actions workflow to automatically build and test a Python application.

app.py

def add(a, b):
    return a + b

def greet(name):
    return f"Hello, {name}!"
  
test_app.py

from app import add, greet

def test_add():
    assert add(2, 3) == 5

def test_greet():
    assert greet("World") == "Hello, World!"
    
requirements.txt

pytest


.github/workflows/python-test.yml


name: Python Build & Test

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest test_app.py -v


-----








