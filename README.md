Load Testing Rest API With Jenkins/K6/Grafana
This repository has the files that were used to perform the load test on a CRUD api example from gorest.co.in.

Procedure For Testing
Setup api token for gorest.co.in
Test the rest api using Postman
Export the postman collection to a json file
Convert the postman collection to a k6 script using postman-to-k6
Run k6 script manually and verify that it works
Start docker instance with jenkins
Create a pipeline job to trigger the jenkins pipeline
Create a repository in git
Add jenkins pipeline to execute the performance test
Configure connectivity between jenkins and git
Setup Webhook from git to jenkins to trigger the build
Create Jenkins Pipeline job to monitor changes in git
Run the pipeline and verify that it works
Run docker container with influxdb and grafana
Integrate k6 with influxdb/grafana
Push changes to the sample project to trigger the pipeline
Verify that the build is triggered automatically
View the k6 results via jenkins
View the results in grafana
Import grafana dashboard for more metrics and rerun the test
Description of procedure
The procedure starts with obtaining an api token for gorest.co.in. The site has 3 options for generating a token. Using the api token, we have to validate the api functionality using a GUI/command line tool such as Postman.

In Postman, a collection was created for each of the 4 request types. The sample get request used for the exercise does not require authorization, but the other 3 - POST/PUT/DELETE - do need a bearer token to be configured for authentication.

After successful verification of the API methods using postman, the collection is exported and uploaded to the jenkins enabled load testing environment. Using postman-to-k6 - installed via npm - the postman collection is converted to a k6 load test script.

The load test script is updated for any configuration parameters, such as the number of virtual users. It is a good idea to run the load test script manually with 1 VU (virtual user) to verify that the script works properly.

The next step is to setup a jenkins environment. To speed up the process, a Docker container running jenkins can be used for this purpose.

The objective was to initiate execution of the load tests on any updates to a git repo. The Jenkins file and load test scripts were uploaded to a git repo.

The git repo also contains the Jenkins pipeline to execute the necessary steps to execute the load test using k6.

A pipeline project is created in Jenkins to retrieve the artifacts from the git repository. The pipeline project also includes steps to execute the pipeline based on a commit to the git repo.

Whenever a change is committed to the git repo, the webhook configured on the git repo notifies Jenkins to trigger the build of the pipeline project. The jenkins project also needs to have the poll scm option enabled.

The git repo has a script for installing k6 on the jenkins node. Since these commands run as root using sudo, it is necessary to configure the jenkins docker container to allow the jenkins user to use the sudo command. This is a one time setup that needs to be done.

Once the integration between git and Jenkins is validated, the next step is to setup the reporting environment - consisting of influxdb + grafana. Docker compose can be used for this purpose - allowing rapid setup of a dashboard.

k6 can use the --out parameter to send load test metrics to the influxdb timeseries database. Grafana supports multiple dashboards that can render the metrics from the influxdb data store.
