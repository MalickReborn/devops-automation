# devops-automation
Prerequisites:
 - docker installed on the host and access to a docker hub account
 - jenkins
 - git for version control

Setting up your environment:
Install the Docker Pipelines plugin on Jenkins:
Manage Jenkins → Manage Plugins

Search Docker Pipelines, click on Install without restart and wait until it is done.

Add Credentials.
In Jenkins you have to add a new credential with your Docker Hub account details. Go to Credentials → Global → Add credentials and fill out the form with your username and password. 
For this test instead of user password i opted for a token based authentification
you can get it generated in your dockerhub account --- account settings > security > new access token
then reach your manage jenkins credentials global add credential to register your dockerhub username and token as password
[![Screenshot-from-2023-10-17-15-14-50.png](https://i.postimg.cc/7YDd79Xv/Screenshot-from-2023-10-17-15-14-50.png)](https://postimg.cc/t7SBGhLD)


  3. Create your Jenkins pipeline.
Now, we will create our Pipeline then type the name you want for this Pipeline project and then click OK. 
[![Screenshot-from-2023-10-17-15-02-07.png](https://i.postimg.cc/d3719vJ1/Screenshot-from-2023-10-17-15-02-07.png)](https://postimg.cc/8fSN12JQ)
[![Screenshot-from-2023-10-17-15-02-27.png](https://i.postimg.cc/28CqJqCp/Screenshot-from-2023-10-17-15-02-27.png)](https://postimg.cc/kDhXRgVw)
You have to add github credentials and give your github repository url. And save and apply it. Here you have to write your dockerfile for your project ( an exemple in the repo folder). It will be based on your application properties.


 4. And for running jenkins pipeline here we will write a jenkinsfile
which defines the pipeline. Please note you need to modify the jenkinsfile according to your details. (see the edited jenkinsfile into the repo)

Now we are ready to run the Pipeline and check the output if an error is present on any stage during the run.

Go to your Pipeline project on Jenkins and click on Build Now to run manually. You should get a sequential output of the different stages similar to this one:
[![Screenshot-from-2023-10-17-15-43-09.png](https://i.postimg.cc/8cB6vN46/Screenshot-from-2023-10-17-15-43-09.png)](https://postimg.cc/q6qqVf9J)

You can also see console output:
[![Screenshot-from-2023-10-17-16-13-17.png](https://i.postimg.cc/g04D19Zd/Screenshot-from-2023-10-17-16-13-17.png)](https://postimg.cc/zLbKRc7P)

If everything is fine, you can check your Docker Hub repository for a new image tagged with the Jenkins build version matching with your Docker Hub registry:
[![Screenshot-from-2023-10-17-15-18-46.png](https://i.postimg.cc/CLgT7ShQ/Screenshot-from-2023-10-17-15-18-46.png)](https://postimg.cc/CdmQLyfG)

Jenkins file
If it successfully pushes its well and good. Otherwise you might face an error. When I was applying for it I faced a problem. That was the Docker Permission Problem. I got the below error message while running the Jenkins job: 

`ERROR: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/_ping": dial unix /var/run/docker.sock: connect: permission denied`

To address this problem execute the below command on your Jenkins server terminal.
* if you encounter some issues with your jenkins enable to reach the docker deamon:
  - create group Docker if not already existing
    `sudo groupadd docker`
  - add jenkins to the docker group
      `sudo usermod -a -G docker jenkins`
  - restart jenkins and docker service
     `sudo systemctl daemon-reload
      sudo systemctl restart docker
      sudo service jenkins restart`
  - if after a new attempt issue is still occuring , reboot the host
    `reboot`
     

