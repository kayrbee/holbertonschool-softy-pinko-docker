## Front-end
For this task, start by making a copy of your task1 directory and name it task2. We have created a very simple API server with one route that returns “Hello, World!” and now we want to create a web page to view content from our API server in the context of a more full front-end. Before creating our front end, let’s reorganize this project a bit in our task2 directory.

Create a new directory named back-end inside of your task2 directory.

Move all of the files currently in task2 inside of the new back-end directory.

> You should have api.py and Dockerfile inside of your new task2/back-end directory.

Create a new task2/front-end directory

Inside your new task2/front-end, clone this repository -> https://github.com/atlas-school/softy-pinko-front-end

With the softy-pinko-front-end directory and files in place, we need to create a new Dockerfile in your task2/front-end directory.

In order to host and distribute our front-end content we will use a static web server named Nginx; there are many others that can be used, but this one is rather simple to get started with and conveniently has a Docker image that we can use which is pre-installed.

In the new task2/front-end/Dockerfile, instead of using the latest ubuntu version, use the latest version of nginx.

Your softy-pinko-front-end files must be copied to /var/www/html/softy-pinko-front-end on the Docker image.

Create an Nginx config file named softy-pinko-front-end.conf inside of the task2/front-end directory. This file must be copied to the Docker image to /etc/nginx/conf.d/default.conf and must include all of the configuration settings required to get your site to show up when visiting the URL.

> When researching Nginx config files, the only section you’ll need in the softy-pinko-front-end.conf file is the “server” section. Pay attention to the syntax used to set up a port to listen to (recommendation: port 9000), the name of the server, the location, and the index file to use.

softy-pinko-front-end.conf
```
server {
// Replace with your Nginx server configuration
}
```
At the end of this process, you should have a front end that is accessible like the following: