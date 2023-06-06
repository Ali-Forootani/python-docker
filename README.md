# python-docker






# Prerequisites

    You understand basic Docker concepts.
    You’re familiar with the Dockerfile format.
    You have enabled BuildKit on your machine.
# Overview

Now that you have a good overview of containers and the Docker platform, let’s take a look at building your first image. An image includes everything needed to run an application - the code or binary, runtime, dependencies, and any other file system objects required.

To complete this tutorial, you need the following:

    Python version 3.8 or later. Download [Python](https://www.python.org/downloads/)
    Docker running locally. Follow the instructions to download and [install Docker](https://docs.docker.com/desktop/)
    An IDE or a text editor to edit files. We recommend using [Visual Studio Code](https://code.visualstudio.com/Download).

# Sample application

The sample application uses the popular Flask framework.

Create a directory on your local machine named python-docker and follow the steps below to activate a Python virtual environment, install Flask as a dependency, and create a Python code file.

```bash cd /path/to/python-docker
python3 -m venv .venv
source .venv/bin/activate
(.venv) $ python3 -m pip install Flask
(.venv) $ python3 -m pip freeze > requirements.txt
(.venv) $ touch app.py
```


Add code to handle simple web requests. Open the python-docker directory in your favorite IDE and enter the following code into the app.py file.
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, Docker!'
```


# Test the application

Start the application and make sure it’s running. Open your terminal and navigate to the working directory you created.

```bash 
cd /path/to/python-docker
source .venv/bin/activate
(.venv) $ python3 -m flask run
```

To test that the application is working, open a new browser and navigate to `http://localhost:5000`


Switch back to the terminal where the server is running and you should see the following requests in the server logs. The data and timestamp will be different on your machine.

`127.0.0.1 - - [22/Sep/2020 11:07:41] "GET / HTTP/1.1" 200 -`




## Create a Dockerfile for Python


Open an empty text file and copy the following:

```
#syntax=docker/dockerfile:1

FROM python:3.8-slim-buster

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

CMD ["python3", "-m" , "flask", "run", "--host=0.0.0.0"]
```
## Directory structure
Just to recap, you created a directory on your local machine called `python-docker` and created a simple Python application using the Flask framework. You used the `requirements.txt` file to gather requirements, and created a Dockerfile containing the commands to build an image. The Python application directory structure should now look like the following:

```
python-docker
|____ app.py
|____ requirements.txt
|____ Dockerfile
```


## Build an image

Now that you’ve created the Dockerfile, let’s build the image. To do this, use the `docker build` command. The `docker build` command builds Docker images from a Dockerfile and a “context”. A build’s context is the set of files located in the specified PATH or URL. The Docker build process can access any of the files located in this context.

The build command optionally takes a `--tag` flag. The tag sets the name of the image and an optional tag in the format `name:tag`. Leave off the optional tag for now to help simplify things. If you don’t pass a `tag`, Docker uses “latest” as its default tag.

Build the Docker image.


```bash
 docker build --tag python-docker .
```


## View local images

To list images, run the `docker images` command.

```bash
sudo docker images
```
You should see at least one image listed, including the image you just built `python-docker:latest.`

