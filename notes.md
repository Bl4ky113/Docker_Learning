# Docker Learning
Straight from the Docs, Just like my coffe, plain with nothing else, than Coffe.

Start:
- 01/29/2023

Sessions:
- 01/29/2023

## What is Docker, and Why?

Docker is a Plataform for Dev, Ship and Run Apps on our Infrastructure, so we can ship it to production more quickly.
They offer ways to shipping, testing and deploying code more quickly.

Docker Works on Containers of Code, these can be used from a host easily, beeing light and just what you need to 
run your app. These can be conected to others containers, which form a orchestated service. 

Using Docker, we can push and code more quickly, by just pushing a Docker Container Image to 
Production, making the process way faster.
Thanks to its lightweight implementation, we can scale our docker containers easily.
And the very same lightweight capability, help us by reducing power, processing and
general using less resources.

Docker Works by mainly a daemon which, is the one who mantains the docker app. By taking from a 
registry, an image, which is a template for a container, where after creating one, we are going to 
use our app. This can be made by using different docker customization files in our project.

## Make an App an Container

We are going to pull a node.js app to test and learn. 

To containerize the app, we go to its folder in repo/app. We create a Dokerfile, which contains:

- The syntaxt declarator
- The image which is going to use, a nodejs one
- set the work dir
- the contents that are going to be copied
- a command which will run once the container is created, setup
- The same that before? Run the app
- Export the 3000 port of the container

Then we run docker cli

$ docker build -t image-name repo/app

This will create the docker container, and since it's the firt time that we run it, it will download the
node.js image. Then we name it after the parameter of -t.

### Making Docker Work
In My pc, I had some problems running docker, 
the first was systemctl had it dissabled and not running. 
Then, docker need root permissions to download the nodejs image.
After that, it works like a charm.

We made the container, but for running it we will use the cli again

$ docker run -dp 3000:3000 image-name

This will run the image-name container that we made before, and will take the 
3000 port that the image was exporting and map it to our 3000 port.

We can test going in localhost:3000 

We can check the docker containers that we are running with the cli

\# docker ps

## Simple Management of Docker Containers

If we change our app, the change wont show in our test. So we need to 
remake the container right? Yea we could, but we first need to 
stop the container using its id.
We can check its id with ps, and then after all that process, 
we need to remake the container and then
we can run again the container.

## Share your Images

For Sharing our Docker Images we need to create an account in Docker. So 
I think that I'll skip this step by now.
But I'll read it.

We can create a new repo for our image, then we can run a docker command to 
send the image to the repo.

\# docker push userid/repo-name:tagname

But first we might need to login in our account from the docker cli.
And then tag the image from the cli

\# docker tag image-name userid/repo-name

then, docker should let us push our image to the repo.

From DockerHub, we can even try our image by using a new instance. But this is just 
a virtual machine connected via our browser, so it's the "same".

