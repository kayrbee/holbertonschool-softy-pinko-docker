## Orchestration
Having separate Docker images/containers per component of your application can reduce the complexity of your web apps. That said, having more than a single Docker image that you need to spin up in containers can present challenges; what if you need to spin up 3 distinct Docker images, or 7, or 50? Opening each one, one at a time would quickly become an issue. That’s where Docker Compose comes into play. With a docker-compose.yml file, we can specify the different components of your entire application, set up some basic configurations, and allow Docker to handle spinning up the entire application all at once, no matter how many containers there are.

Before going further, be sure to copy the task3 directory and name it task4.

Inside the task4 directory, create a docker-compose.yml file

Once you’ve set up your docker-compose.yml file with the correct services, you should be able to build them using docker-compose build and run it with docker-compose up like this: