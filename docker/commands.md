## Run particular version of software
36mins
### Run latest
For example, let's run latest version of `redis`
```
docker run redis
```
### Run particular version
For example, let's run v4.0 of `redis`
```
docker run redis:4.0
```

## Run your application which accepts input
In this example the flag _i_ stands for input and _t_ stands for pseudo terminal.
```
docker run -it <your image>
```

## PORT Mapping
Every docker has it's own individual IP (Internal IP : only available inside docker) assigned. But you can access your 
web-page using the docker host IP address by mapping the host port (for example 80) to your port (for exmaple 5000) 
running inside docker.
```
docker run -p 80:5000 <image>
```

## Volume Mapping (Storage)
For example, if you're running `mysql` image on a docker then all the data stored is local to the docker and gets erased
as soon as the docker image is destroyed.
```
// Run mysql
docker run mysql

// Do some work

// Destroy image, and all data is lost
docker stop mysql
docker rm mysql
```
To persist data in docker host, you need to map the storage path. Let's assume the following -
```
Docker Storage Path : /var/lib/mysql
Docker Host Path : /opt/datadir
```
Then to map these path(s) run the following command to store all your data inside your host -
```
docker run -v /opt/datadir:/var/lib/mysql mysql
```

## Inspect Container
The result would be in a JSON format.
```
docker inspect <container-name>
```

## Container logs
In case you've run an image in detach mode using `-d` flag, then to check the logs you can use the following command -
```
docker logs <container-name>
```

## Environment variables

### Set value
In case you're using environment variables like `os.environ.get('<key>')` in your code, then you can change the values
at runtime without changing code.
```
docker run -e <key>=<value> <image>
```

### Check value for running container
```
docker inspect <container-name>
```
Under the `Config/Env` section, you'll find the `key:value`.

## Docker Image

### Create your own image
For example, let assume you're running a simple web application which requires the following steps -
1. OS-Ubuntu
2. Update apt repo
3. Install dependencies using apt
4. Install python using pip
5. Copy source code to /opt folder
6. Run the web server using "flask" command

Now given the steps, we can add these instructions in one docker file -

Dockerfile [Instruction(FROM, RUN, COPY, etc) Argument]
```
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```
Now build the image locally out of this file using this command -
```
docker build Dockerfile -t tag-name
```
To publish and make it available publicly -
```
docker push tag-name
```
You can check the layered-achitecture history of the image created by running command -
```
docker history <image-name>
```
Docker will cache the progress of build for the image.