pipeline {
  agent any

  triggers {
    cron('H/5 * * * 1')   
  }

  options { timestamps() }

  stages {
    stage('Checkout') {
      steps { checkout scm }
    }

    stage('Build + Test') {
      steps {
        bat 'gradlew.bat clean test'
      }
      post {
        always {
          junit 'build/test-results/test/*.xml'
        }
      }
    }

    stage('Jacoco Coverage Report') {
      steps {
        bat 'gradlew.bat jacocoTestReport'
      }
    }

    stage('Package Artifact') {
      steps {
        bat 'gradlew.bat build -x test'
      }
    }
  }

  post {
    success {
      archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
      archiveArtifacts artifacts: 'build/reports/jacoco/test/html/**', allowEmptyArchive: true
    }
  }
}
