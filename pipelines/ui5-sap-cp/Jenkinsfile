#!groovy

import com.sap.piper.Utils

/**
 *	Copyright (c) 2017 SAP SE or an SAP affiliate company.  All rights reserved.
 *
 *	This software is the confidential and proprietary information of SAP
 *	("Confidential Information"). You shall not disclose such Confidential
 *	Information and shall use it only in accordance with the terms of the
 *	license agreement you entered into with SAP.
*/

@Library('piper-library-os') _

node() {
  //Global variables:
  APP_PATH = 'src'
  SRC = "${WORKSPACE}/${APP_PATH}"
  CONFIG_FILE = "${SRC}/.pipeline/config.properties"

  stage("Clone sources and setup environment"){
    deleteDir()
    def gitCoordinates = new Utils().retrieveGitCoordinates(this)
    checkout([$class: 'GitSCM', branches: [[name: gitCoordinates.branch]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: APP_PATH]], submoduleCfg: [], userRemoteConfigs: [[url: gitCoordinates.url]]])
    dir(APP_PATH) {
      setupCommonPipelineEnvironment script: this, configFile: CONFIG_FILE
    }
    MTA_JAR_LOCATION = commonPipelineEnvironment.getConfigProperty('MTA_HOME')
    NEO_HOME = commonPipelineEnvironment.getConfigProperty('NEO_HOME')
    DEPLOY_HOST = commonPipelineEnvironment.getConfigProperty('DEPLOY_HOST')
    CI_DEPLOY_ACCOUNT = commonPipelineEnvironment.getConfigProperty('CI_DEPLOY_ACCOUNT')
    neoCredentialsId = commonPipelineEnvironment.getConfigProperty('neoCredentialsId')
    proxy = commonPipelineEnvironment.getConfigProperty('proxy') ?: ''
    httpsProxy = commonPipelineEnvironment.getConfigProperty('httpsProxy') ?: ''
  }

  stage("Validate configuration"){
    echo '[INFO] Validating configuration.'
    if (!MTA_JAR_LOCATION) error "The property 'MTA_HOME' is not configured. Please configure the property at '$CONFIG_FILE'."
    if (!NEO_HOME) error "The property 'NEO_HOME' is not configured. Please configure the property at '$CONFIG_FILE'."
    if (!DEPLOY_HOST) error "The property 'DEPLOY_HOST' is not configured. Please configure the property at '$CONFIG_FILE'."
    if (!CI_DEPLOY_ACCOUNT) error "The property 'CI_DEPLOY_ACCOUNT' is not configured. Please configure the property at '$CONFIG_FILE'."
    if (!neoCredentialsId) error "The property 'neoCredentialsId' is not configured. Please configure the property at '$CONFIG_FILE'."
    echo '[INFO] The validation was successful.'
  }

  stage("Validate installations"){
    echo '[INFO] Validating installation requirements.'
    toolValidate tool: 'java', home: JAVA_HOME
    toolValidate tool: 'mta', home: MTA_JAR_LOCATION
    toolValidate tool: 'neo', home: NEO_HOME
    echo '[INFO] The validation was successful.'
  }

  stage("Build Fiori App"){
    dir(SRC){
      withEnv(["http_proxy=${proxy}", "https_proxy=${httpsProxy}"]) {
        MTAR_FILE_PATH = mtaBuild mtaJarLocation: MTA_JAR_LOCATION, buildTarget: 'NEO'
      }
    }
  }
  
  stage("Deploy Fiori App"){
    dir(SRC){
      withEnv(["http_proxy=${proxy}", "https_proxy=${httpsProxy}"]) {
        neoDeploy script: this, neoHome: NEO_HOME, archivePath: MTAR_FILE_PATH
      }
    }
  }
}


