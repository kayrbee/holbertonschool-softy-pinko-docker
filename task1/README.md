## Back-end
For this task, start by making a copy of your task0 directory and name it task1. Next, we want to change the Dockerfile to install Python3, pip3, and Flask. You may not have used Flask, yet, but not to worry; for this project, we will give you all of the Flask code you need to get started. We’ll validate that all have been installed correctly by running a Flask server with one endpoint that when called returns “Hello, World!”

Install python3, python3-pip, and flask

Note: Make sure to automatically install and skip user input with the -y flag for apt-get
Note: flask must be installed with pip3, not through apt-get
If you get a This environment is externally managed error when trying to install Python packages, add the following line before calling pip on your Dockerfile

> RUN rm /usr/lib/python*/EXTERNALLY-MANAGED

Locally, create a Python file named api.py and paste the following Python script - it uses Flask to create one endpoint that returns “Hello, World!” when called

Hosting this Flask app on 0.0.0.0 instead of 127.0.0.1 means that it is reachable outside of the current machine (the current machine being a Docker container which is running inside of your laptop/desktop)
Host this Flask app on port 5252
api.py

from flask import Flask

app = Flask(__name__)

@app.route('/api/hello')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5252)
In your Dockerfile, use /app as the working directory and copy the Python file to your Docker image

When running your Docker image, your Flask server should spin up and accept requests

You will need to make sure that you forward the Docker container’s port 5252 to the host machine’s port 5252 when running your image in a container.