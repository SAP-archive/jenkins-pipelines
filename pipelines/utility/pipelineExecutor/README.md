# Pipeline for loading and executing another pipeline

The `Jenkinsfile` in this folder is part of project "Piper". It materializes another pipeline script and executes that pipeline script. This is especially useful if several projects should be built in the same way. In this case the executed pipeline script referenced by this executor pipeline script can be maintained centrally and used by all the projects.

To use it, the executor pipeline must be contained in the source repository of the applications that is to be built. The coordinates of the executed pipeline need to be provided in the project's configuration file (default ```.pipeline/config.properties```).

# Prereqisites

To run the executor pipeline script, you need to configure the [shared library](http://github.com/SAP/jenkins-library) in your Jenkins master.

# Parameters

The following parameters need to be maintined in the project configuration.

* ```JENKINSFILE_GIT_URL``` - The repository containing the pipeline script to be executed.
* ```JENKINSFILE_GIT_BRANCH``` - The branch inside the repository mentioned above which holds the pipeline script.
* ```JENKINSFILE_PATH``` - The path to the pipeline script to be executed.
* ```JENKINSFILE_GIT_CREDENTIALS_ID``` - The credentialsId of the repository mentioned above. Only required in case that repository requires authentification. The corresponding credentials needs to be maintained in Jenkins.


