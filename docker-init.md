## What is docker init?

docker init is a command-line utility that helps in the initialization of Docker resources within a project. It creates Dockerfiles, Compose files, and .dockerignore files based on the project's requirements.

This simplifies the process of configuring Docker for your project, saving time and reducing complexity.

*Latest version of docker init supports Go, Python, Node.js, Rust, ASP.NET, PHP, and Java. It is available with Docker Desktop.*

## How to use docker init?

Using docker init is easy and involves a few simple steps. First, go to the directory of your project where you want to set up Docker assets.

Let me Create a basic Flask app.

```
touch app.py requirements.txt
```

Copy the below code in the respective files

```
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_docker():
    return '<h1> hello world </h1'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
# requirements.txt
Flask
```

### Let's see the magic of docker init

*docker init will scan your project and ask you to confirm and choose the template that best suits your application. Once you select the template, docker init asks you for some project-specific information, automatically generating the necessary Docker resources for your project.*

```
docker init
```

![image](https://github.com/user-attachments/assets/126d84f7-f4ee-49cd-baaa-4e4d548f8d82)

The next thing you do is choose the application platform, for our example we are using python. It will suggest the recommended values for your project such as Python version, port, entrypoint commands.

![image](https://github.com/user-attachments/assets/25607be7-f628-45cd-83df-0e36685bc28a)

You can either choose the default values or provide the values you want, and it will create your docker config files along with instructions for running the application on the fly.

![image](https://github.com/user-attachments/assets/5daf9e2d-ed78-4322-9d2f-9c3f5f51e137)
