#BlueMix, Docker Container, DevOps Service Demo
This demo to show how to develope a native Java application and build by maven and deply to Bluemix by  Bluxmix DevOps service. There are some setting on Bluemix DevOps Service.
This project has the following module
- WebService maven module - Which is only a Java Dynamic Applcation.
- DockerWebServer module - which only help to store the Dockerfile and nothing more.
- Docker module - that is a main module include both two modules.

##Step 1 commit your Java dynamic web project into GitHub
##Step 2 commit the Dockerfile to DockerWebServer module under docker folder
```docker
FROM tomcat:8.0
EXPOSE 8080
COPY *.war /usr/local/tomcat/webapps
```
##Step3 Setup the IBM Bluemix DevOps Servies 
Go to web page for [BlueMux DevOps](https://hub.jazz.net/) Setting
Create an account if not.

###Step3.1 Create Project for DevOps
Click Create Project
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249094/9a777082-faaa-11e5-918d-1896e60c469b.png)
Name as Docker2
Select the Link to an existing Github repository
Select Link to a repo on GitHub
Select the link from GitHub. You may need to link the github account on first time use.
Once the account create you can click the project and it a repo show

