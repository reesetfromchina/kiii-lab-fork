pipeline {
  agent any
  stages {
    stage('Clone repository') {
      steps {
        checkout scm
      }
    }

    stage('Build image') {
      steps {
        script {
          def app
          app = docker.build("navipro/kiii")
          ddocker.withRegistry('https://registry.hub.docker.com', 'dockerhub', "${DOCKER_CREDENTIALS}") {
            app.push("${env.BRANCH_NAME}-${env.BUILD_NUMBER}")
            app.push("${env.BRANCH_NAME}-latest")
            // signal the orchestrator that there is a new version
          }
        }

      }
    }

  }
  environment {
    DOCKER_CREDENTIALS = credentials('dockerhub')
  }
}