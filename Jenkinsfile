#!groovy

import com.sap.piper.Utils
import com.sap.piper.ConfigurationLoader
import com.sap.piper.ConfigurationMerger

/**
 *	Copyright (c) 2017 SAP SE or an SAP affiliate company.  All rights reserved.
 *
 *	This software is the confidential and proprietary information of SAP
 *	("Confidential Information"). You shall not disclose such Confidential
 *	Information and shall use it only in accordance with the terms of the
 *	license agreement you entered into with SAP.
*/

@Library('piper-library-os') _

CONFIG_FILE_PROPERTIES = '.pipeline/config.properties'
CONFIG_FILE_YML = '.pipeline/config.yml'

def isNewConfigFramework(def configFile) {
    return configFile.equals(CONFIG_FILE_YML)
}

node() {
  //Global variables:
  APP_PATH = 'src'
  SRC = "${WORKSPACE}/${APP_PATH}"

  def CONFIG_FILE

  def STEP_CONFIG_NEO_DEPLOY='neoDeploy'
  def STEP_CONFIG_MTA_BUILD='mtaBuild'

  stage("Clone sources and setup environment"){
    deleteDir()
    Map neoDeployConfiguration, mtaBuildConfiguration
    dir(APP_PATH) {
      checkout scm
      if(fileExists(CONFIG_FILE_YML) ) {
          CONFIG_FILE = CONFIG_FILE_YML
      } else if(fileExists (CONFIG_FILE_PROPERTIES) ) {
          CONFIG_FILE = CONFIG_FILE_PROPERTIES
      } else {
          error "No config file found."
      }
      echo "[INFO] using configuration file '${CONFIG_FILE}'."
      setupCommonPipelineEnvironment script: this, configFile: CONFIG_FILE
      prepareDefaultValues script: this
      neoDeployConfiguration = ConfigurationMerger.merge([:], (Set)[],
                                                         ConfigurationLoader.stepConfiguration(this, STEP_CONFIG_NEO_DEPLOY), (Set)['neoHome', 'host', 'account', 'neoCredentialsId'],
                                                         ConfigurationLoader.defaultStepConfiguration(this, 'neoDeploy'))
      mtaBuildConfiguration = ConfigurationMerger.merge([:], (Set)[],
                                                        ConfigurationLoader.stepConfiguration(this, STEP_CONFIG_MTA_BUILD), (Set)['mtaJarLocation'],
                                                        ConfigurationLoader.defaultStepConfiguration(this, 'mtaBuild'))
    }
    MTA_JAR_LOCATION = mtaBuildConfiguration.mtaJarLocation ?: commonPipelineEnvironment.getConfigProperty('MTA_HOME')
    NEO_HOME = neoDeployConfiguration.neoHome ?: commonPipelineEnvironment.getConfigProperty('NEO_HOME')
    DEPLOY_HOST = neoDeployConfiguration.host ?: commonPipelineEnvironment.getConfigProperty('DEPLOY_HOST')
    CI_DEPLOY_ACCOUNT = neoDeployConfiguration.account ?: commonPipelineEnvironment.getConfigProperty('CI_DEPLOY_ACCOUNT')
    neoCredentialsId = neoDeployConfiguration.neoCredentialsId ?: commonPipelineEnvironment.getConfigProperty('neoCredentialsId')
    proxy = commonPipelineEnvironment.getConfigProperty('proxy') ?: ''
    httpsProxy = commonPipelineEnvironment.getConfigProperty('httpsProxy') ?: ''
  }

  stage("Validate configuration"){

    def newConfig =  isNewConfigFramework(CONFIG_FILE)

    echo '[INFO] Validating configuration.'
    if (!MTA_JAR_LOCATION) {
        if(newConfig) {
            error "The property 'mtaJarLocation' is not configured inside step configuration '$STEP_CONFIG_MTA_BUILD'. Please configure the property at '$CONFIG_FILE'."
        } else {
            error "The property 'MTA_HOME' is not configured. Please configure the property at '$CONFIG_FILE'."
        }
    }
    if (!NEO_HOME) {
        if(newConfig) {
            error "The property 'neoHome' is not configured inside step configuration '$STEP_CONFIG_NEO_DEPLOY'. Please configure the property at '$CONFIG_FILE'."
        } else {
            error "The property 'NEO_HOME' is not configured. Please configure the property at '$CONFIG_FILE'."
        }
    }
    if (!DEPLOY_HOST) {
        if(newConfig) {
            error "The property 'host' is not configured inside step configuration '$STEP_CONFIG_NEO_DEPLOY'. Please configure the property at '$CONFIG_FILE'."
        } else {
            error "The property 'DEPLOY_HOST' is not configured. Please configure the property at '$CONFIG_FILE'."
        }
    }
    if (!CI_DEPLOY_ACCOUNT) {
        if(newConfig) {
            error "The property 'account' is not configured inside step configuration '$STEP_CONFIG_NEO_DEPLOY'. Please configure the property at '$CONFIG_FILE'."
        } else {
            error "The property 'CI_DEPLOY_ACCOUNT' is not configured. Please configure the property at '$CONFIG_FILE'."
        }
    }
    if (!neoCredentialsId) {
        if(newConfig) {
            error "The property 'neoCredentialsId' is not configured inside step configuration '$STEP_CONFIG_NEO_DEPLOY'. Please configure the property at '$CONFIG_FILE'."
        } else {
            error "The property 'neoCredentialsId' is not configured. Please configure the property at '$CONFIG_FILE'."
        }
    }
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


