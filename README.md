# OKD/OCP Jenkins Pipeline Examples

## Prerequesites 

- a working OKD/OCP (see minishift or crc for test environment)
- a project namespace
- a jenkins instance deployed within the project namespace (not for Basic NGINX build)

## Basic NGINX build 

This example shows a basic external webhook from github after building a NGINX instance with the
"basic-html-page" directory. 
This scenario shows how to:
- setup a "prod" environment based on the master branch
- setup a "dev" environment based on the env-dev branch
- both have webhook from github that trigger a build on git push events
- have a very simple concept of CI/CD pipeline without any extra tools

Fork the repo and perform the following adapting the repo URL:
![preview](https://raw.githubusercontent.com/rovandep/ocp-nodejs-pipeline/master/images/basic-html-page-01.gif)
![preview](https://raw.githubusercontent.com/rovandep/ocp-nodejs-pipeline/master/images/basic-html-page-02.gif)
![preview](https://raw.githubusercontent.com/rovandep/ocp-nodejs-pipeline/master/images/basic-html-page-03.gif)
![preview](https://raw.githubusercontent.com/rovandep/ocp-nodejs-pipeline/master/images/basic-html-page-04.gif)
![preview](https://raw.githubusercontent.com/rovandep/ocp-nodejs-pipeline/master/images/basic-html-page-05.gif)

Repeat for a second project targeting the env-dev branch and create a specific webhook for push event.  
These steps will provide a "build-on-push" scenario as described above.

However, in a real world scenario, no push event should happen on the master branch but when the env-dev branch
is mature and tested enough, a pull request followed by merge to the master should happen. 
That's the actual last event that should trigger the build for the master/prod.

To do so, modify the first create webhook for "Let me select individual events". Leaving it to your research to 
make it work (no modification to the URL or secret, just the individual events).

## Simple nodejs example

This example is a quick and dirty way to show the usage of a buildconfig YAML file to create a 
Build Config within OKD/OCP that will use a Jenkins instance within the namespace to build
a simple ephemeral nodejs & mongodb instances with a working external route. 

To use it, it can be done through CLI: 
``` 
# oc create -f https://raw.githubusercontent.com/rovandep/ocp-nodejs-pipeline/master/nodejs-pipeline.yaml
# oc get buildconfig
NAME                     TYPE              FROM   LATEST
nodejs-pipeline          JenkinsPipeline          6

# oc start-build nodejs-pipeline
# oc get build
NAME                       TYPE              FROM          STATUS     STARTED          DURATION
nodejs-pipeline-1          JenkinsPipeline                 Complete   13 minutes ago   
``` 

It can be done through GUI too:
or from the project namespace Status or in Build Configs, click "Add, Import YAML" and copy/paste 
the contain of "nodejs-pipeline.yaml"

Once added, you can click the "Start Build" which will redirect you to the Build details. From that page,
the Jenkins pipeline job overview can be observed or click on "View Logs" to open Jenkins. 
