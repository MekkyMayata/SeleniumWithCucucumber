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
          // ensure plugin exists
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

    stage('Notification') {
      steps {
        script {
          info()
        }
          // send to email
        emailext (
          subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
            <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
          recipientProviders: [[$class: 'DevelopersRecipientProvider']]
        )
      }
    }

  }
}
def info() {
  echo "Hello World"
}