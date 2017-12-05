# Stub pipeline script for loading another pipeline

The `Jenkinsfile` in this folder is part of project "Piper". It materializes another pipeline script and executes that pipeline script. This is especially usefull if several projects should be built in the same way. In this case the pipeline script referenced by this stub pipeline script can be maintained centrally and used by all the projects.

To use it, you need to checkin the stub pipeline into the sources of the applications that needs to be build. The coordinates of the central pipeline needs to be provided in the projects configuration file (default ```.pipeline/config.properties```. The stub pipeline script is some kind of template. It works like it is provided, but it can be adjusted if needed.

# Prereqisites

To run the stub pipeline script, you need to configure the [shared library](http://github.com/SAP/jenkins-library) in your Jenkins master.

# Parameters

The following parameters needs to be maintined in the project configuration.

* ```JENKINSFILE_GIT_URL``` - The repository containing the referenced pipeline script.
* ```JENKINSFILE_GIT_BRANCH``` - The branch inside the repository mentioned above which holds the pipeline script.
* ```JENKINSFILE_PATH``` - The path to the pipeline script.
* ```JENKINSFILE_GIT_CREDENTIALS_ID``` - The credentialsId of the repository mentioned above. Only required in case that repository requires authentification. The corresponding credentials needs to be maintained in Jenkins.


