pipeline {
  agent any
  parameters {
    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
  }
  stages {
    stage('Initialize') {
      steps {
        echo "Initialize ${params.PERSON}"
        input message: 'Ready to go?', parameters: [choice(choices: ['dev', 'prod'], description: '', name: 'branch')]
        echo "${choice}"
      }
    }
    stage('Build') {
      steps {
        echo 'Build'
      }
    }
    stage('Upload') {
      steps {
        echo 'Upload'
      }
    }
    stage('Report') {
      steps {
        echo 'Report'
      }
    }
  }
}