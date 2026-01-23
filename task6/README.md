## Scale horizontally

Congratulations! You’ve got a proxy server that routes to either a static-content server or to an API server for dynamic data. This works well, for now…but what would happen if traffic to your site spiked up dramatically to where your API server becomes too flooded with requests to be able to respond quickly? In this case, we would like to add a second API server and load-balance all requests between those two servers. Luckily, with Nginx and Docker, this is a simple flag when running docker-compose up!

For this task, make a copy of the task5 directory and name it task6. Your goal is to launch your Docker containers such that you have 2 (or more) API servers. Nginx will act as a load-balance between them so that every request goes to the next API server using the Round-Robin load-balancing algorithm.

Create a new file in the task6 directory named 2-api-servers.txt and put in the exact docker-compose command used to spin up two API servers, instead of just one. Note: for this task, the checker will assume that you named your back-end server back-end and your file must end with a newline Your output should look like this: