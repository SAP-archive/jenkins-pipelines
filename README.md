# Description

An efficient software development process is vital for success in building
business applications on SAP Cloud Platform or SAP on-premise platforms.
SAP addresses this need for efficiency with project "Piper". The goal of project
"Piper" is to substantially ease setting up continuous deployment processes for
the most important SAP technologies by means of Jenkins pipelines.

Project "Piper" consists of two parts:

 * A [shared library][piper-library] containing steps and utilities that are
   required by Jenkins pipelines.
 * A set of [Jenkins pipelines][piper-pipelines] using the piper library to
   implement best practice processes.

## What you get

This repository contains Jenkins pipeline instructions in form of a
`Jenkinsfile` (together with some auxiliary files) which require the [shared
library][piper-library] of project "Piper" to run. They are available under
`pipelines/<scenario>/Jenkinsfile` in this repository.

These best practice Jenkinsfiles are based on the general concepts of [Pipelines
as Code with Jenkins][jenkins-doc-pipelines]. They give you the power of the
Jenkins community at hand to optimize your continuous delivery processes.

You can run the Jenkinsfiles out of the box or take them as a starting point for
project-specific adaptations.

## Overview of available pipelines

Currently, the following scenarios are supported:

* [The build and deployment of a SAPUI5 or SAP Fiori application to SAP Cloud
  Platform][piper-pipelines-fiori-doc].

# Requirements

* [Installed shared library][piper-library-installation] of project "Piper".
* Jenkins User with privileges to create jobs.

# Download and Installation

There are two recommended ways to consume our `Jenkinsfile`. For both ways, you
need to create a new pipeline job in Jenkins that [retrieves a pipeline from an
SCM][jenkins-doc-pipelineFromSCM]. The location of the `Jenkinsfile` may
differ:

1. The `Jenkinsfile` can be copied into the source code repository of the
   application. The `Jenkinsfile` is referenced in the pipeline job
   configuration.

2. The `Jenkinsfile` can be kept in its own repository separate from the
   application. In case you want to apply the `Jenkinsfile` to multiple
   applications, this approach is beneficial since you only need to maintain the
   `Jenkinsfile` in one place. You can consume the `Jenkinsfile` e.g. directly
   from this repository on GitHub.com or from your own fork of it.
   In this case there is a stub `Jenkinsfile` required in the source repository.
   An ready-to-use stub pipeline script is available
   [here](pipelines/utility/pipelineExecutor/Jenkinsfile)
   ([docu](pipelines/utility/pipelineExecutor/README.md)).

# Configuration

Each `Jenkinsfile` requires project specific configuration files in the
application sources. Templates for these configuration files are available in
the same folders as the respective `Jenkinsfile`. Links to the respective
documentations can be found in the
[pipeline overview](#overview-of-available-pipelines) section.

# Known Issues

A list of known issues is available on the [GitHub issues page of this
project][piper-pipelines-issues].

# How to obtain support

Feel free to open new issues for feature requests, bugs or general feedback on
the [GitHub issues page of this project][piper-pipelines-issues].

# Contributing

Read and understand our [contribution guidelines][piper-pipelines-contribution]
before opening a pull request.

# [License][piper-pipelines-license]

Copyright (c) 2017 SAP SE or an SAP affiliate company. All rights reserved.
This file is licensed under the Apache Software License, v. 2 except as noted otherwise in the [LICENSE file][piper-pipelines-license]

[piper-library]: https://github.com/SAP/jenkins-library
[piper-pipelines]: https://github.com/SAP/jenkins-pipelines
[piper-pipelines-issues]: https://github.com/SAP/jenkins-pipelines/issues
[piper-library-installation]: https://github.com/SAP/jenkins-library/blob/master/README.md#download-and-installation
[piper-pipelines-fiori]: https://github.com/SAP/jenkins-pipelines/blob/master/pipelines/ui5-sap-cp/Jenkinsfile
[piper-pipelines-fiori-doc]: https://github.com/SAP/jenkins-pipelines/blob/master/pipelines/ui5-sap-cp/README.md
[piper-pipelines-license]: ./LICENSE
[piper-pipelines-contribution]: .//CONTRIBUTING.md
[jenkins-doc-pipelines]: https://jenkins.io/solutions/pipeline
[jenkins-doc-libraries]: https://jenkins.io/doc/book/pipeline/shared-libraries
[jenkins-doc-steps]: https://jenkins.io/doc/pipeline/steps
[jenkins-doc-pipelineFromSCM]: https://jenkins.io/doc/book/pipeline/getting-started/#defining-a-pipeline-in-scm
[jenkins-plugin-sharedlibs]: https://wiki.jenkins-ci.org/display/JENKINS/Pipeline+Shared+Groovy+Libraries+Plugin
