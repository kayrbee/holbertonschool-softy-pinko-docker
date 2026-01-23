FROM ubuntu:22.04

# install dependencies
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y python3 python3-pip
RUN pip install flask

# SET working dir & copy the code files
WORKDIR /app
COPY ./api.py /app/api.py

# Configure port
EXPOSE 5252

# Run app inside container
CMD ["flask", "--app", "api.py", "run", "--host", "0.0.0.0", "--port", "5252"]