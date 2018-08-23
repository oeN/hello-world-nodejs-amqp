def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label, containers: [
    containerTemplate(name: 'node', image: 'node:alpine', ttyEnabled: true, command: 'cat'),
    containerTemplate(
      name: 'rabbitmq', 
      image: 'rabbitmq:alpine', 
      ttyEnabled: true,
      livenessProbe: containerLivenessProbe(execArgs: 'rabbitmqctl ping', , initialDelaySeconds: 60, timeoutSeconds: 1, failureThreshold: 3, periodSeconds: 10, successThreshold: 1)
    )
  ]) {

    node(label) {
      stage('Test') {
        container('node') {
          sh 'node --version'
          checkout scm
          sh "yarn install"
          sh "yarn test"
        }
      }

      stage('Logs') {
        containerLog('node')
        containerLog('rabbitmq')
      }

      stage('Deploy') {
        steps {
            echo 'Deploying....'
        }
      }
    }
  }
