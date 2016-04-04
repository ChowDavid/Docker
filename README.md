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
The script above help to copy the war file and Docker file from workspace into root level. The IBM Container build can only lookup the Dockerfile and their resource in roor level. so that the TEMO_PWD is the roor level and everything need to put on the root.
and Dockerfile show above will help to collect the war file into webapp folder inside the image. The IBM container builder will build the image and assign the new tag number for it as below:
```
registry.ng.bluemix.net/davidchow/dockerwebtest   1.0                 5391651c8f44        6 days ago          465 MB
registry.ng.bluemix.net/davidchow/myapp           24                  6562dacc14cd        2 hours ago         357.4 MB
registry.ng.bluemix.net/davidchow/myapp           25                  543e3e2d03fc        2 hours ago         357.4 MB
registry.ng.bluemix.net/davidchow/myapp           26                  1c727674e720        About an hour ago   357.4 MB
registry.ng.bluemix.net/davidchow/myapp           27                  1f71bfd284b1        About an hour ago   357.4 MB
registry.ng.bluemix.net/ibm-mobilefirst-starter   latest              a100524c96cb        13 days ago         921.6 MB
registry.ng.bluemix.net/ibm-node-strong-pm        latest              3e2373877cf5        10 days ago         572 MB
registry.ng.bluemix.net/ibmliberty                latest              33fdda9431c7        2 weeks ago         555.3 MB
registry.ng.bluemix.net/ibmnode                   latest              a4964fd52b4f        11 days ago         434.7 MB
registry.ng.bluemix.net/ibmnode                   v4                  a4964fd52b4f        11 days ago         434.7 MB
registry.ng.bluemix.net/ibmnode                   v1.1                e4812bb29c8e        11 days ago         410.1 MB
registry.ng.bluemix.net/ibmnode                   v1.2                5f50977d9959        11 days ago         426.2 MB
```
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249873/71448bd8-faae-11e5-8d32-0c980d865b43.png)

As the result the Build Stage became

![alt tag](https://cloud.githubusercontent.com/assets/4963861/14249877/751afc74-faae-11e5-93a6-d88e3d147a01.png)

####Step 3.2.2 Create the other stage of DEV Deployment

The input select Input Type Build Artifacts and Stage select the previous stage

![alt tag](https://cloud.githubusercontent.com/assets/4963861/14250465/1f9a36ea-fab1-11e5-9479-5e5adb34cabf.png)

The JOBS select Deploy, The DeployerType select IBM Containers on Bluemix, the Name create a new name for container, the PORT select 8080 it should be same as Dockerfile EXPOSE 8080 above.

![alt tag](https://cloud.githubusercontent.com/assets/4963861/14250466/1f9d6572-fab1-11e5-8965-54737dffcd2d.png)

Then the stage will show now become It may not has the IP bind. The IP should be bind on DASHBOARD. Please select the IP for the container.

![alt tag](https://cloud.githubusercontent.com/assets/4963861/14250462/1f582142-fab1-11e5-80c1-e20517ae4739.png)

![alt tag](https://cloud.githubusercontent.com/assets/4963861/14250455/18709f4e-fab1-11e5-8f6f-219593906eb6.png)


##Step 4 Test the result
The IP was show on the container instance now it is 134.168.27.43 and Port Expose as 8080, the service was define on the Dockerfile and the servlet. it is WebService. We can check the output on the browser

##Step 5 Code Change
Before the code change the DevOps as below
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14251192/67dbecb6-fab4-11e5-93c9-b5e5062a1615.png)

Now change the code for index.html and commit to github then the DevOps will detected the code change and build by itself as below
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14251185/64ae92c8-fab4-11e5-9595-abac490b6182.png)
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14251187/64b158be-fab4-11e5-807c-647b59356b9f.png)
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14251186/64aee002-fab4-11e5-9429-8af3200ce1e0.png)
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14251183/64ab23cc-fab4-11e5-831a-428f9a044f86.png)


after a while the deployment step will also execute and a new instance will deploy to BlueMix w/o setting any thing.
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14251184/64abaa40-fab4-11e5-9bfa-b559c4405538.png)
![alt tag](https://cloud.githubusercontent.com/assets/4963861/14251182/64aac2c4-fab4-11e5-9582-ca350a7c5b67.png)





