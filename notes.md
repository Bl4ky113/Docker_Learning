# Docker Learning
Straight from the Docs, Just like my coffe, plain with nothing else, than Coffe.

Start:
- 01/29/2023
End:
- 01/31/2023

Sessions:
- 01/30/2023
- 01/31/2023

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

## Persistent Data in Docker

Unitl know in our NodeJs app, we save a little list of to dos, but everytime we 
run or recreate the container, the dbs of the project wipes out its content.
And even if we need the data in other containers, we can't access the data from one 
container to other.

We can use volumes to store the data of the container in our local storage, 
and even from there we can share it to other containers.

To do this, we first create the volume by creating it from the cli:

\# docker volume create volume-name

We stop the containers and then run them again mounting the volume.

\# docker run -dp 3000:3000 --mount type=volume,src=volume-name,target=/path/to/data image-name

Then we can try it and see that our data is now persistent. For our example, the target path
is /etc/todos/, where the todo.db is stored in the containers.

I tried, and it works flawless.

But wait, where is this data beeing stored? We can use volume inspect volume-name, to check 
the data of the volume, showing its mountpoint, generally on /var/lib/docker/volumes/volume-name/\_data/

\# docker volume inspect volume-name

## Bind Mounts

Bind Mounts are a way to bind the mounting of an app, to a directory in the local storage.
This way we can set where the data will be store if we use persistent data like volumes. And
If the container changes, it will auto reload its content, avoiding to change all the process that we 
saw, in order to show changes.

But the weird thing is that we need to reinstall, and basicly do to the same stuff than in the Dockerfile, but from
the terminal then we can use the app, and enjoy of the nodemon that help us to restart the container, adding the new
changes. 

We can see if our container has any errors or if it working properly by using the cli logs:

\# docker logs -f container-id

Then when we are over or ready to ship and share the docker container, we can ship it by just building our image.
And then doing the other stuff.

## Multicontainer Apps or Orchestated Services

Apps, more likely in production, never are just one single app with some simple usage. It might have running databases, 
or other services. We can separate these apps or functions in its individual docker container. And it should be like that
since the containers should only contain one thing, and do it well.

Also, other than isolating the process, there's some extra reasons to separate the services in different containers.
- Scability 
- Update and upgrade individualy our services versions
- Set up a testing or development container, for example a dev db or testing db.
- Running multiple process in a single container can add complexity and resource usage to the container

In order to connect the containers we need to add networking to them, if they are in the same network, they can connect and
work together. Create one by using the cli:

\# docker network create network-name

In our example and exercise we are going to create a MySQL db and connect it to our app.
We are going to run an image of MySQL, adding the network name and the name of the DB in the network.

To conect to the db from the docker terminal, we can run mysql from the container, then enter the password, which is "secret".
And test the db.

Then we can connect it to a container, but we need some tools to connect to our container, 
we are going to download and connect to the container network netshoot by nicolaka. A set of tools to connect 
containers.

After downloading the container, we can check with "dig mysql" our container network prefences.
In ANSWER SECTION will show the hostname of our mysql container and its ip.

Then to add our DB to the app, we add to the network, and the MySQL env variables.
We can check again with exec mysql to the db container and query on todos_items, add some items in the app,
and then check them in the db.

## Docker Composer

Docker Composer is a tool that helps us to create our services just once, by writing a YAML file, which is just like a 
JSON like file, where we define the data of our services.

But first, this doesn't come with the docker cli, we have to install it in our pc. In Arch, the pacman Repo has a package ready 
to install. So it is a simple install process.

Now, we are going to the root of our project to create a docker-compose.yml file.
This has this structure:

- Define the list of services or containers in our app
    then inside of it, we will migrate all of the services that we've 
    created until this point.

- For each service or container, we name it, and then add inside it its values such as:
    - image
    - command to run
    - ports to map
    - working-dir
    - volumes
    - enviroment variables
    - ...

When we run docker run with all these values, Docker will detect that some volumes doesn't exist and it creates them, such as 
the ones in the db. But using compose, we cant, so we have to add a volumes values in the base of the yaml file. Creating the 
volume, by just adding its name.

We are ready to start the composer, but first stop and remove all the services and containers of the project that you have running.
Then with:

\# docker compose up -d

It's ready to roll. Well I've had some problems with the MySQL server, but meh, its almost over the tutorial, so I will skip it.

Then when we need to stop it, we just run:

\# docker compose down --volumes

We add the tag, because with the normal down, the volumes aren't deleted.

## Image Building Best Practices.

We can scan our docker images for security. But this functionality are only available if you 
log in in your dockerhub account, and also it uses a 3rd party service, snyk. It will show some 
vulneravilities, which we can fix with help of the software or something, since this is a 
3rd party functionality, we are not going to use it a lot.

The images gets build by layers, a way to see how are these, or what are, we can use the image history 
from the cli:

\# docker image history image-name
And we can add --no-trunc to see the full output of the command, but it is pretty long.

Now, we know that in our image, the always copy and paste all the node-modules from our project, 
and then for each start or use of the image delete the modules and then repeat.
Althought is fairly fast, we can make it faster.

We can first copy the packages.json file, where the modules are stored, run the install of the 
packages, and then copy the files of our project.
But even now, docker keeps copying the modules, so we need to add an .dockerignore file, 
where we are going to add the node\_modules folder.

Now, it will build more faster than before, and even more if we rebuild the image, like for example.
We made a change, and re build the image.

We can also make multiple-stage builds which are more for apps and projects which need a more
complex building. Where we can use multiples images using FROM and giving them an alias with the
keyword as custom-name.

In NodeJS and Javascript projects, we generally just compile Typescript, SASS, or other framework 
into a more static html, css and js project. So we can push or copy the output into a server or a 
NGINX server.

## The End, now What?

This is only the beggining. But we can learn and do so much more, 
we can use and learn different tools such as:

- Container Orchestadors:
    - Kubernetes
    - Swarm
    - Nomad
    - ECS

We can deep dive in how implement all of this in a cloud service with CNCF projects.

We also could deep dive into how Docker works using Go, by watching some videos, :p. 

And That's All of the tutorial, go on, and kill a beast with this info.
