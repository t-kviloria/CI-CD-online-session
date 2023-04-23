pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        script {
          checkout scm
          def customImage = docker.build("${registry}:${env.BUILD_ID}")
        }

      }
    }
    stage('unit-test'){
      steps{
        script{
          docker.image("${registry}:${env.BUILD_ID}"). inside {c -> sh 'python app_test.py'}
        }
      }
    }
    stage('http-test'){
      steps{
        script{
          docker.image("${registry}:${env.BUILD_ID}"). withRun('-p 9006:9000') {c -> sh "curl -i http://localhost:9006/test_string"}
          }
      }
    }

    stage('Publish') {
      steps {
        script {
          docker.withRegistry('', 'dockerhub-id') {
            docker.image("${registry}:${env.BUILD_ID}").push('latest')
          }
        }

      }
    }

  }
  environment {
    registry = 'itemo/ci-cd-online-workshop'
  }
}
