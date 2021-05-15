pipeline {
  agent any
  
  environment {
    branch = 'master'
  }
  stages {
    stage('SCM Checkout') {
      steps {
        script {
          notifyStarted()
        }
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
// ++++++++++++++++methods+++++++++++++++++++++++++++++++
def notifyStarted() {
  echo "hello world"
  // send to Slack
  // slackSend (color: '#FFFF00', message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

  // send to HipChat
  // hipchatSend (color: 'YELLOW', notify: true,
      // message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
    // )

  // send to email
  emailext (
      subject: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
      recipientProviders: [[$class: 'DevelopersRecipientProvider']]
    )
}
// def notifyBuild(String buildStatus = 'STARTED') {
//   // build status of null means successful
//   buildStatus = buildStatus ?: 'SUCCESS'

//   // Default values
//   def colorName = 'RED'
//   def colorCode = '#FF0000'
//   def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
//   def summary = "${subject} (${env.BUILD_URL})"
//   def details = """<p>STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
//     <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>"""

//   // Override default values based on build status
//   if (buildStatus == 'STARTED') {
//     color = 'YELLOW'
//     colorCode = '#FFFF00'
//   } else if (buildStatus == 'SUCCESS') {
//     color = 'GREEN'
//     colorCode = '#00FF00'
//   } else {
//     color = 'RED'
//     colorCode = '#FF0000'
//   }

//   // Send notifications
//   slackSend (color: colorCode, message: summary)

//   hipchatSend (color: color, notify: true, message: summary)

//   emailext (
//       subject: subject,
//       body: details,
//       recipientProviders: [[$class: 'DevelopersRecipientProvider']]
//     )
// }