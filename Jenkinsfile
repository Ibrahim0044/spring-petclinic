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
        sh './gradlew clean test'
      }
    }

    stage('Jacoco Coverage Report') {
      steps {
        sh './gradlew jacocoTestReport'
      }
    }

    stage('Package Artifact') {
      steps {
        sh './gradlew build -x test'
      }
    }
  }

  post {
    success {
      archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
      archiveArtifacts artifacts: 'build/reports/jacoco/test/html/**', allowEmptyArchive: true
      junit 'build/test-results/test/*.xml'
    }
  }
}
