## Repo: docker-demo
Docker - an introductory, hands-on demo

Ever written code and given it to someone and then they say they can't run it because:
- their OS setup or environment variables are different than yours
- their version of Python is different
- they're using different Python libraries than you and with different versions of each
  etc. etc.

Wouldnt it be nice if you could give them an exact copy of your computer/environment so that they can run 
it and see what you see?  THAT is where Docker's container technology comes in.

With Docker, you can create an image of the platform and everything running on it, including your python
code, your database version, and anything else that's needed.  Send the image to them (a relatively small 
file that runs very quickly) and you and them are now confident that your environmental setups are the same.  

Now, real team technical development can begin because you are both operating in environments that are clones
of one another -- all because they started the process by both using the same Docker image.

This repo demos how to get up and running with Docker.

## Getting Started

# Preliminaries - Linux setup and downloads.

1. Open a Linux session.

    If you dont have Linux machine you can spin up an AWS EC2 instance with a free AWS account. 
      - https://docs.aws.amazon.com/quickstarts/latest/vmlaunch/welcome.html
        - download the private key (*.pem file) 
        - copy the public IP address of the EC2 instance

    Connect to your EC2 instance's Ubuntu session using MobaXterm's (or PuTTy's) SSH session  
        - to run SSH, download software like MobaXterm: https://mobaxterm.mobatek.net/
        - use the public IP of EC2 instance and *.pem file* from EC2 instance to connect to your EC2's Ubuntu (Linux) session. 

    At this point, you will now have a Unix machine running, one way or the other.

2. Download docker to Linux.
    
    The following are commands you can run at your Unix prompt:
    
    sudo -s                 # run as superuser mode; you can skip this step if you want and then come back to it if you get an error running 'apt' command.
    apt update              # update your linux machine's advanced package tool, 'apt' before using it in next steps
    apt install docker.io   # this installs docker from web, from URL docker.io
    
    That's it.  You can now verify Docker is loaded on your machine:
    
    which docker            # expected result something like:  /usr/bin/docker
    docker --version        # expected result something like: 19.03.6, build 39ce7...   Another test to verify that you now have docker available on your Linux machine.
    
    At this point, you now have Unix machine with Docker installed on it.


## Now, the fun stuff...! 

3. Download Docker images from Docker Hub.

    The most popular Docker images are already created and available online, here: https://hub.docker.com/search/?type=image
      
      - Python, Java, Perl, Ruby, Groovy, Julia, etc.
      - MySQL, Postgres, Oracle Enterprise Edition, etc.
    
      ...There are 5 million images.  To cut down on the noise, filter on "Official Images" in the webpage's 
      left menubar and then, say, "Programming Langauges."  Python unsurprisingly is listed first, being 
      very popular.  https://hub.docker.com/_/python


    You can also search through images through the Unix command line:
    
        docker search python    # the most popular image  "<image>:<tag aka version>" is listed at the top of available python images
        docker search ubuntu    # yes you can even run a container that has its own unix - possibly a different version than your host machine - running inside it.
    
    Here is a video of a guy doing just this: https://youtu.be/-tHeJbGP8e0?t=95
    
    To pull the image you have chosen to your machine, just type:
    
        docker pull python         # get the latest version.  The tag or suffix ":latest" is assumed. 

        # You can come back to these details later:
        
        docker pull python:latest  # does same as 'docker pull python'
        docker pull python:3.6     # gets specific version (you may not want the absolute latest)
        docker pull python:2.7     # gets specific version (could be a much older version python)
   
  
   Pull a couple other images if you want.  
    
        docker pull mysql
        docker pull postgres
        docker pull perl
   
   Now, to list your inventory of images you have:
   
        docker images     

    By the way, for help:

        docker help                 # for general help on the most popular commands.
        docker images --help        # for specific help on a given command, in this case 'docker images'


4. Run the container.

        docker run -it --rm python      # The "-it" runs in interative mode. The "--rm" just cleans up container nicely after you exit python.
        

        
## Further Exercises      

# Lab 1: Running Python in a Docker Container  ("Hello World")

    We'll run a container, enter it, and happily code and run programs from INSIDE the container, oblivious to the outside world.


    Here is what I did:

      root@ip-172-7-25-155:~/blair# docker run -it --rm python            
      Python 3.9.1 (default, Feb  9 2021, 07:42:03)
      [GCC 8.3.0] on linux
      Type "help", "copyright", "credits" or "license" for more information.
      >>> print("hello world from inside a docker container")
      hello world from inside a docker container
      >>> print (3 + 4)
      7
      >>> exit()
      root@ip-172-31-25-160:~/blair#             <--- I'm now back outside the container at Unix prompt.


    ...We can now pack more stuff into the container:
      - more Python libraries including specifying a Python requirements.txt file, 
      - add a database engine (eg mysql or postgres  -- create tables, then load them from Python, say.), 
      - additional programming language (if you get bored of Python or if you want to race them)
      - a man cave ...really, whatever we want.   
      
    We can then save the state of the container as a new standalone Docker image. 
    
    If we exit the container and stop that instance, it forgets everything.  So committing it's state 
    to a new image is imperative if you want to share
    

# Lab 2: Running Python in a Docker Container  ("Hello World", part II)
  
    We'll now run a container, but interact with code running inside the container while we remain OUTSIDE the container.

    Here is a simple github with code.  
    https://github.com/rskTech/k8s-demo/tree/main/docker

    You will need to manually 'pip install flask' insider your container as the Python code uses Flask.
    
    This mini Python application is also now starting to emulate how a container might be deployed 
    as part of a production system with different components talking to each other from within the
    same container or even between containers.
    

# Lab 3: Further Reading and Building Docker images from scratch (Intro to Dockerfiles)

    The little detail above about 'pip install flask' (since our Python script required flask library) raises
    the need to be able to script your container so that not only Python core library is loaded but also any
    library (pandas, numpy, flask, etc) in your requirements.txt file can also get loaded so it's all ready
    for you or anybody else that runs that script (aka "Dockerfile") that is used to build your Docker image.

  Start here, for more on Dockerfiles:
    https://blog.realkinetic.com/building-minimal-docker-containers-for-python-applications-37d0272c52f3
    
  Basic Docker commands
    - https://www.youtube.com/watch?t=95&v=-tHeJbGP8e0&feature=youtu.be
  
  Creating "Dockerfile" to script your Docker container's creation (and/or can be used to build it's image from scratch)
    - https://www.youtube.com/watch?v=LQjaJINkQXY
    
  Intermediate "Dockerfile" examples at someone's github:
    - https://github.com/opencord/voltha/tree/master/docker    


# Beyond Docker

    Docker can do more than I show above.  But let's look even beyond those unvisited features.
    
    Docker assumes you are operating container(s) on a single machine. In distributed (multiple machine) environments, 
    you need a little more oomph and that's where Kubernetes steps in, orchestrating containers in various ways.
    
    For more on Kubernetes, head to Youtube videos on the topic or head to: https://kubernetes.io/
    
