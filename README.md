# OCP Jenkins Pipeline with nodejs and mongodb images

as-is this OCP buildconfig template will create the necessary Jenkins pipeline 
to build a set of nodejs and mongodb pods accessible externally via a OCP route.

``` 
oc create -f https://raw.githubusercontent.com/rovandep/ocp-nodejs-pipeline/master/nodejs-pipeline.yaml
``` 

This will create the buildconfig within the current project space.
If you wish to create it in a different project space, edit namespace attribute in the file.

