# dockertestingenvironment
How To Configure a Continuous Integration Testing Environment with Docker and Docker Compose on Ubuntu 16.04

Create a fresh folder for our application by executing:
```
cd ~
mkdir hello_world
cd hello_world

```
Edit a new file app.py with nano:
```
nano app.py

```
Add the following content:

```
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host="redis")

@app.route("/")
def hello():
    visits = redis.incr('counter')
    html = "<h3>Hello World!</h3>" \
           "<b>Visits:</b> {visits}" \
           "<br/>"
    return html.format(visits=visits)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=80)
    
```
-
Add the contents in requirements.txt :

```
nano requirements.txt

Flask
Redis
```
-

```
nano Dockerfile
```
Add the following contents:

```

FROM python:2.7

WORKDIR /app

ADD requirements.txt /app/requirements.txt
RUN pip install -r requirements.txt

ADD app.py /app/app.py

EXPOSE 80

CMD ["python", "app.py"]

```

Edit a new file:

```
nano docker-compose.yml

```
Add the following contents:

```
web:
  build: .
  dockerfile: Dockerfile
  links:
    - redis
  ports:
    - "80:80"
redis:
  image: redis
```
The docker-compose.yml and Dockerfile files allow you to automate the deployment of local environments by executing:

```
docker-compose -f ~/hello_world/docker-compose.yml build
docker-compose -f ~/hello_world/docker-compose.yml up -d
```
