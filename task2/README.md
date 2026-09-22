# Objective

## Create a Docker image for a simple front-end application

In task 1 we created a very simple API server with one route that returns “Hello, World!” and in task 2 we want to create a simple frontend web page. Instead of writing our own frontend, we'll be copying an example frontend called `softy-pinko`. 

> This task focuses only on setting up a frontend server in a Docker container; connecting the FE to our backend API is out of scope until task 3. 

## Task setup

- Create a new directory named `backend` inside of your `task2` directory.
- Copy all of your task 1 files into the `task2/backend` directory. 
    - At this point you should have `api.py` and `Dockerfile` inside of your new `task2/backend` directory.
- Create a new `task2/frontend` directory
- Inside your new `task2/frontend` directory, clone this repository -> https://github.com/atlas-school/softy-pinko-front-end
- With the softy-pinko-front-end directory and files in place, create a new Dockerfile in your `task2/frontend` directory.

**Task 2 directory tree structure**
```
task2/
├── backend/
│   ├── api.py
│   └── Dockerfile
└── frontend/
│   ├── softy-pinko-front-end/ # contains cloned files from the softy-pinko project
    └── Dockerfile
```

## Task instructions

In order to host and distribute our front-end content we will use a static web server named `Nginx`; there are many others that can be used, but this one is simple to get started with and conveniently has a Docker image that we can use. 

> This task does involve doing a little bit of research into nginx server configuration, but I've included a helper in the notes section if you get stuck.

### Steps

1. In the new `task2/frontend/Dockerfile`, write a line that uses the latest version of nginx as the image (instead of using the latest ubuntu version as we did in task 1).

2. Write a line in your Dockerfile that will copy your softy-pinko-front-end files to `/var/www/html/softy-pinko-front-end` on the Docker image.

3. Create a new file named `softy-pinko-front-end.conf` inside of the `task2/frontend` directory (at the same level as the Dockerfile). This file is a config file, and must include all of the nginx configuration settings required to get your site to show up when visiting the URL.

    > When researching Nginx config files, the only section you’ll need in the softy-pinko-front-end.conf file is the “server” section. Pay attention to the syntax used to set up a port to listen to (recommendation: port 9000), the name of the server, the location, and the index file to use.

    ```
    # softy-pinko-front-end.conf

    server {
    // Replace with your Nginx server configuration
    }
    ```

4. Write a line in your Dockerfile that will copy the nginx config file you created in step 3 into the Docker image at file path `/etc/nginx/conf.d/default.conf` 

5. Build & run both of your Dockerfiles in separate terminals

    ```
    # From terminal A
    cd task2
    docker build -t task2-frontend frontend/.
    docker run -it --rm -p 9000:9000 --name web task2-frontend

    # From terminal B
    cd task2
    docker build -t task2-backend backend/.
    docker run -it --rm -p 5252:5252 --name api task2-backend
    ```

At the end of this process, you should have:

- A backend that is accessible at `http://localhost:5252/api/hello` which returns `Hello, World!`. 
- A front end that is accessible at
`http://localhost:9000` and looks like:

    ![alt text](image.png)

Right now, the frontend and the backend are running in separate containers, which are not configured to connect to each other. In task 3, we'll be wiring the two containers up to communicate dynamically. 
