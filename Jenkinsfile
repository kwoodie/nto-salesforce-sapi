pipeline {

  agent any
  environment {
    //adding a comment for the commit test
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.3.0'
    BG = "Test"
    WORKER = "Micro"
  }
  stages {
    stage('Build') {
      steps {
      configFileProvider(
        [configFile(fileId: 'global-maven-settings', variable: 'MAVEN_SETTINGS')]) {
            sh 'mvn -s $MAVEN_SETTINGS -B -U -e -V clean -DskipTests package'
        }
            
      }
    }
    
    
    stage('Deploy Development') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'nto-salesforce-sapi'
      }
      steps {
            
            sh 'mvn -U -V -e -B -DskipTests deploy -DmuleDeploy -Dmule.version="${MULE_VERSION}" -Danypoint.username="${DEPLOY_CREDS_USR}" -Danypoint.password="${DEPLOY_CREDS_PSW}" -Dcloudhub.app="${APP_NAME}" -Dcloudhub.environment="${ENVIRONMENT}" -Dcloudhub.bg="${BG}" -Dcloudhub.worker="${WORKER}"'
      }

    }
     
  }

  tools {
    maven 'M3'
  }
}