pipeline {
  agent {
    kubernetes {
      label "mypod-${UUID.randomUUID().toString()}"
      yamlFile 'KubernetesPod.yaml'
    }
  }

  stages {
    stage('Test') {
      steps {
        container('node') {
          sh 'node --version'
          checkout scm
          sh "yarn install"
          sh "yarn test"
        }
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying....'
      }
    }
    stage('Logs') {
      steps {
        containerLog('node')
        containerLog('rabbitmq')
      }
    }
  }
}
