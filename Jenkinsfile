pipeline {
  agent any
  
  environment {
    branch = 'master'
  }
  stages {
    stage('SCM Checkout') {
      steps {
        git branch: branch, url: 'https://github.com/MekkyMayata/SeleniumWithCucucumber.git'
      }
    }
    stage('Test') {
      parallel {
        stage('Maven') {
          steps {
            echo 'Running from Jenkins file'
            sh(script: 'mvn compile', label: 'maven')
          }
        }

        stage('Cucumber') {
          steps {
            cucumber '**/*.json'
          }
        }

      }
    }
    stage('Clean workspace on condition') {
      steps {
        cleanWs cleanWhenSuccess: false, cleanWhenNotBuilt: true, cleanWhenAborted: true, cleanWhenUnstable: true, cleanWhenFailure: true
      }
    }

  }
}
