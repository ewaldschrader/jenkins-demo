def gv

pipeline {
  agent any 
  
  parameters{
    choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
    booleanParam(name: 'executeTests', defaultValue:true, description: '')                                
  }
  
  stages {
    stage("init") {
      steps {
        script {
          gv = load "script.groovy"
        }
      }
    }
    
    stage("build") {
      steps {
        script {
          gv.buildApp()
        }
      }
    }
    stage("test") {  
      when {
        expression {
          params.executeTests
        }
      }
      steps {
        script {
          gv.testApp()
        }
      }
    }
  stage('SonarQube analysis') {
      def scannerHome = tool 'SonarScanner 4.0';
      withSonarQubeEnv('My SonarQube Server') { // If you have configured more than one global server connection, you can specify its name
        sh "${scannerHome}/bin/sonar-scanner"
      }
    }
    stage("deploy") { 
      steps {
        echo "deploying version ${params.VERSION}"
        script {
          gv.deployApp()
        }
      }
    }
  }
}

