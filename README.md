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
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249375/0aa5e8b0-faac-11e5-97b5-363fb6dc44a2.png)
Name as Docker
Select the Link to an existing Github repository
Select Link to a repo on GitHub
Select the link from GitHub. You may need to link the github account on first time use.
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249384/0ec4e158-faac-11e5-8b90-382e51b32ee0.png)
Once the account create you can click the project and it a repo show
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249391/14a5a72e-faac-11e5-995f-9fb868a46683.png)
Now the GitHub was binded to bluemix. Once the github code changed The hub.jazz also know.
###Step 3.2 Click on the Top Right button BUILD & DEPLOY
####Step 3.2.1 Create Build State

Click the Add Stage. 
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249849/559b67da-faae-11e5-8971-c92335491426.png)

Then Select the input type as SCM Repository
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249870/659de4d2-faae-11e5-88f8-81476d0177ec.png)

Click the Jobs tab to Add Job. Test and Tester Type as Simple. On the Test Command as below
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249872/6b09fbe0-faae-11e5-91cb-c431b6817037.png)

Then create Build Job, Builder Type is Maven 
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249939/c1388068-faae-11e5-89f7-bbadd7ad888c.png)

Then create Build Job again. This time create Docker Image, Builder type select IBM container service, image name as myapp. It has a default build script. Please add the following scipt below the first time as below
```
# check the previous target
log_and_echo "$LABEL" "Check previous build target"
TEMP_PWD=`pwd`
cd $TEMP_PWD/WebService
mvn -B package
mkdir $TEMP_PWD/DockerWebServer/target
cp $TEMP_PWD/WebService/target/*.war $TEMP_PWD
cp $TEMP_PWD/DockerWebServer/docker/* $TEMP_PWD
cd $TEMP_PWD
ls -al
```
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249873/71448bd8-faae-11e5-8d32-0c980d865b43.png)

As the result the Build Stage became
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249877/751afc74-faae-11e5-93a6-d88e3d147a01.png)


