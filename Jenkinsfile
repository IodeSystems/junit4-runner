pipeline {
  agent any
  stages {
    stage('Build Parameters') {
      when {
        branch "main"
      }
      steps {
        script {
          properties([parameters([booleanParam(defaultValue: false,
                  description: 'Deploy',
                  name: 'DEPLOY')])])
        }
      }
    }

    stage('Build Project') {
      when {
        expression {
          return params.DEPLOY != true
        }
      }
      steps {
        sh 'bin/mvn clean install'
      }
    }

    stage('Build and Publish') {
      when {
        branch 'main'
        expression {
          return params.DEPLOY == true
        }
      }
      steps {
        sh 'bin/dev release'
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }
}
