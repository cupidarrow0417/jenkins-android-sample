pipeline {
  agent any
  parameters {
    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    choice(choices: 'prod', description: '', name: 'branch')
  }
  stages {
    stage('Initialize') {
      steps {
        echo 'Initialize ${params.PERSON}'
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