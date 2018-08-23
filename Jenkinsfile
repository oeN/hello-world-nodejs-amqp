pipeline {
  agent any

  stages {
    stage('Build') {
        steps {
            echo 'Building..'
        }
    }

    stage('Test') {
      steps {
        checkout scm

        echo "Testing: ${params.tag}"

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

    stage('Deploy') {
        steps {
            echo 'Deploying....'
        }
    }
  }
}
