# Objective

## Connect the FE & BE

This task will have you connect your frontend to the backend allowing you to have dynamic data on your frontend. This means that communication will occur between your two Docker images (each of which will be running in their own Docker container). To facilitate this, be sure to have multiple terminal instances open so you can have one Docker container running on each.

## Task instructions

The first thing to do is to copy the `/backend` and `/frontend` directories and files from `task-2` into `task-3`.

Then we need to update the frontend html with a Javascript function that calls the backend api endpoint.

**Frontend changes**

1. Inside your `index.html` file, search for the `<h1>` tag that contains the text “We provide the best strategy to grow up your business”.

2. One line above the 'We provide the best strategy to grow up your business' tag, insert a new line and copy the `<h1>` tag given below. This new tag is the place we’re going to insert dynamic data from our API server.

    ```
    <h1 id="dynamic-content">Dynamic content</h1>
    ```
3. Now we need to actually use some JavaScript code to make the request. Place the following script inside a `<script>` tag at the bottom of `index.html` - put it near the bottom of the file as the last `<script>` tag before the closing tag.

    **New Javascript function to add**
    
    This function requests data from the backend api endpoint, and inserts the response payload as plain text inside the html element with `id=dynamic-content`, which then allows the browser to render the content.

    ```
    // Load dynamic data from the back-end on port 5252
    $(function() {
        $.ajax({
            type: "GET",
            url: "http://localhost:5252/api/hello",
            success: function(data) {
                console.log(data);
                $('#dynamic-content').text(data);
            }
        });
    });
    ```

That is all the frontend needs to be able to connect to the backend, however there is one more thing we must do to our backend so that it can accept cross-origin requests from our frontend. We must use the CORS plugin for Flask.

**Backend changes**

4. In your backend’s `backend/Dockerfile`, use pip3 to install flask-cors, after you have installed flask through pip3. 

5. Next, replace the contents of `api.py` with this updated script.

    ```api.py
    from flask import Flask
    from flask_cors import CORS

    app = Flask(name)
    CORS(app)

    @app.route('/api/hello')
    def hello_world():
        return 'Hello, World!'

    if name == 'main': app.run(host='0.0.0.0', port=5252) 
    ```
    
Now, your back-end will allow your front-end to communicate with it even though they are not on the same server (they are on two different Docker containers).
