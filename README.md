# docker-meteor-example with Deploy onto AWS 

[Project Demo](http://ec2-54-234-89-147.compute-1.amazonaws.com/)

## How to Dockerize it?

Follow a great tutorial for Docker &amp; Meteor on [How to dockerize a Meteor 1.4+ app](https://medium.com/@isohaze/how-to-dockerize-a-meteor-1-4-app-120a34089ddb
).

### Create and Build Meteor APP 
```
$ meteor create ecs && cd ecs
```
Since we pull Node base image, first we need to build meteor app into node one
```
$ meteor build ../build
```
Unzip build file and change the directory to bundle (bundle is the root directory of source code)
```
$ cd ../build && tar -zxvf ecs.tar.gz && cd bundle
```

### Write Dockerfile
In bundle folder, create empty Dockerfile (touch is the easiest way to create new, empty file)
```
$ touch Dockerfile
```
Here is the Dockerfile source code
```Dockerfile
# Pull base image
FROM node:4.4.7

# Copy files at /bundle to container's filesystem at /bundle
# COPY <src> <dest> => Copy files from <src> and add them to the container's filesystem at path <dest>
COPY . /bundle

# Change directory and install dependencies
RUN (cd /bundle/programs/server && npm install)

# Config ENV value
ENV PORT=80

# Expose to port 80 
# EXPOSE informs Docker that the container listens on the specified network ports at runtime.
EXPOSE 80

# Run 
CMD node /bundle/main.js
```

### Build and Run Dockerfile
```
docker build -t meteor/ecs .
docker run -d -p 8080:80 -e ROOT_URL=http://example.com meteor/ecs
```
## How to Deploy it? 
Follow the tutorial [Deploying a MeteorJS app to ECS](http://krishamoud.me/deploying-meteor-to-aws-ecs/)

## DONE! 
:tada::tada::tada::tada:
