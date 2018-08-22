pipeline {
  agent none
  stages {
    stage('Test') {
      agent any
      steps {
        checkout scm

        echo "Testing: ${params.tag}"
        script {
          stage('Checkout Tag') {
            sh "git checkout ${params.tag}"
            sh "git pull || exit 0"
          }

          docker.image('rabbitmq').withRun { rabbitmq ->
            docker.image('nodejs').inside("--link=${rabbitmq.id}:rabbitmq") {
              stage ('Run Tests') {
                withEnv(['RABBITMQ_HOST=rabbitmq']) {
                  sh "yarn install"
                  sh "yarn test"
                }
              }
            }
          }
        }
      }
    }
  }
}
