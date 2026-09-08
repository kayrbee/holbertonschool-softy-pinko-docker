## Objective

### Add a simple back-end application to your Docker image

A Docker container is useful because it can run an application on any host machine, such as your Windows PC, your Mac, or in the cloud, and you don't need to worry about operating system compatibility. 

In this task, we'll write a simple Python API using a Python web framework called Flask, and add the tools and code to our Docker image. In the running container, we’ll validate that everything has been installed correctly by calling the `/api/hello` endpoint on the Flask server and checking that it returns “Hello, World!” as a response.

You may not have worked with Python or Flask before, but not to worry; for this project, you have been given all of the code you need to get started. The learning focus is on ensuring that the Docker image includes all of the  tooling and code that it needs to run a simple API server.

**Task instructions**

1. For this task, start by copying `task-0/Dockerfile` to `task-1/Dockerfile`. 

2. Inside the `task-1` directory, create a Python file named `api.py` and paste the following Python script inside it.


    ```py
    from flask import Flask

    app = Flask(__name__)

    @app.route('/api/hello')
    def hello_world():
        return 'Hello, World!'

    if __name__ == '__main__': app.run(host='0.0.0.0', port=5252)
    ```

    **Notes**

    The script uses Flask to create a simple backend application with one API endpoint that returns “Hello, World!” when called.

    Hosting this Flask app on 0.0.0.0 inside the container instead of the default 127.0.0.1 means that it will be reachable outside of the current machine (the current machine being a Docker container which is running inside of your laptop/desktop). 
    
    Host this Flask app on container port 5252, as specified in `api.py`. You will need to ensure that you forward the container’s port 5252 to the host machine’s port 5252 (aka map the container port to the same port on your PC) to be able to reach the URL.

3. Modify your Dockerfile to:

    - install `python3`, `pip3`, and `flask`
    - set the working directory to `/app`
    - copy the `api.py` file into the image


    **Notes** 

    - Make sure to pass the `-y` flag to all `apt-get` commands to skip user input, otherwise installs will fail
    - `flask` must be installed with `pip3`, not through `apt-get`

4. Build & run your new image 
    
    Note that the `-p` flag sets port forwarding from container to host at runtime, and the `--name` flag sets a user-defined container name. These settings can also be controlled from Docker Desktop once the image has been built.
    
    ```
    docker build -t hello-task-1 .

    docker run -it --rm -p 5252:5252 --name hello-python hello-task-1
    ```
    **Troubleshooting**

    If you get an error saying `This environment is externally managed` during the `docker build` step when trying to install Python packages, add the following line before calling pip on your Dockerfile:

    ```
    RUN rm /usr/lib/python*/EXTERNALLY-MANAGED
    ```


6. Test the `/api/hello` endpoint

    > When running your Docker image, your Flask server should spin up and accept requests. Calls to the `/api/hello` endpoint should return 'Hello, World!'

    From your browser:
    - http://localhost:5252/api/hello
    - http://127.0.0.1:5252/api/hello

    Or from the command line, use curl to reach the same endpoint

**Useful resources**
    
- Examples of Flask Dockerfiles: https://docs.docker.com/reference/samples/flask/ 
- Check the [Dockerfile docs](https://docs.docker.com/reference/dockerfile/) to learn how to set a working directory and copy files

**Extension activities**

- What's the difference between `host=0.0.0.0` and `host=127.0.0.1`? 
