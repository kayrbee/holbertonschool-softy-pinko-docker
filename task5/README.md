## Proxy Server
So far we’ve created a static server and one API server, each of which has its own Docker images and runs in different Docker containers. This is okay, but this means that we have to keep track of two separate locations. If we change the port that the API server is using, then we’d have to change the client to hit that new port; in this case ports on our computer, but it would be IP addresses if these were external servers. This is not ideal, especially if you have many different clients that you’d need to update and deploy.

Instead, let’s put a server in front of our static server and API server that acts as a proxy between the client and our full application. Clients (in this case the web browser) will only have to know about the proxy server’s location and our front-end can also simply hit the same proxy server, rather than going to the API directly, to then be routed to the API server in order to get dynamic data from it.

Before going further, copy the task4 directory and name it task5. In the task5 directory, create a new folder named proxy.

Just like with our static-content server, we’re going to use Nginx for this proxy server. In your proxy directory, create a Dockerfile that uses the latest version of nginx. Also, create a file named proxy.conf, which will contain all of the configuration for your proxy server. In the Dockerfile, make sure to copy the proxy.conf file to this specific location in your Docker image for the proxy server: /etc/nginx/conf.d/default.conf.

For the proxy.conf file, the only section you’ll need in the proxy.conf file is the “server” section. You want this proxy server to be listening to port 80. Set up a location section for / and use proxy_pass to route any request coming in on that route to http://INSERT-YOUR-FRONT-END-DOCKER-COMPOSE-SERVICE-NAME-HERE:9000. Next, set up a location section for /api and use proxy_pass to route any request coming in on that route to http://INSERT-YOUR-BACK-END-DOCKER-COMPOSE-SERVICE-NAME-HERE:5252.
```
proxy.conf

server {
    // Replace with your Nginx server configuration
}
```
Note that you are using the same service name that you used in your docker-compose.yml file for the front and back ends. This is because Docker has an internal DNS that will setup routes to internal IP addresses for those names. This is a bit of “magic” that Docker does, but it makes it super convenient to not have to know the exact IP addresses of these services.

Another thing that needs to be modified is the JavaScript that is used. It was previously calling the back-end server directly; instead, we want to have the JavaScript make an API call through the proxy server as well. Replace your JavaScript at the bottom of the index.html file with the following:
```
JavaScript At Bottom Of index.html


    // Load dynamic data from the back-end on port 5252
    $(function() {
        $.ajax({
            type: "GET",
            url: "/api/hello",
            success: function (data) {
                console.log(data);
                $('#dynamic-content').text(data);
            }
        });
    });
```
Next, we need to update the docker-compose.yml file to add a new service named proxy. Be sure to include the same sections as previously used:

build, context, dockerfile

image

ports

Map the container’s port 80 to the host machine’s port 80.
Note: You should remove the mapping for the front-end and back-end service to the host machine ports. You will want to have port 5252 and port 9000 open internally on the containers so that other Docker containers can reach them, but you do not want external clients reaching out directly to the front-end or the back-end; everything should go through the proxy server. If you do not map any ports, you will not be able to get past the firewall that Docker has put up. Docker uses something similar to Linux’s Uncomplicated Firewall ufw; you do not directly work with this firewall, but instead, map allowed ports in Dockerfiles or docker-compose files to allow/disallow access through certain ports.
depends_on