pipeline {
  agent any
  tools {
    nodejs 'node-16.3.0'
  }
  options {
    timeout(time: 2, unit: 'MINUTES')
  }

  stages {
    stage('Install dependencies') {
      steps {
        sh 'npm i'
      }
    }

    stage('Run tests') {
      steps {
        sh 'npm t'
      }
    }
    stage('RUN REMOTE') {
      steps {
          build wait: false, job: 'parameterized', parameters: [string(name: 'ROOT_ID', value: '$BUILD_ID')]
      }
    }
  }
}

