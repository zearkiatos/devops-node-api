pipeline {
  agent any
  tools {
    nodejs 'node-16.3.0'
  }
  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  environment {
    ARTIFACT_ID = "caprilespe/devops-node-api:${env.BUILD_NUMBER}"
  }

  stages {
    stage('Build') {
      steps {
        script {
          dockerImage = docker.build "${env.ARTIFACT_ID}"
        }
      }
    }
    stage("Run tests") {
      steps {
        sh "docker run ${dockerImage.id} npm run test"
      }
    }
    stage("Publish") {
      when {
        branch "master"
      }
      steps {
        script {
            docker.withRegistry("", "DockerHubCredentials") {
            dockerImage.push()
          }
        }
      }
    }
    stage("Install dependencies") {
      steps {
        sh 'npm i'
      }
    }
    stage("RUN REMOTE") {
      steps {
          build wait: false, job: 'parameterized', parameters: [string(name: 'ROOT_ID', value: '$BUILD_ID')]
      }
    }
  }
}

