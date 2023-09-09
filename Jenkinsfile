pipeline {
      agent { label 'kubepod'}
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        
        sh 'docker build -t oyinkz/jenkins-docker-hub .'
        
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push oyinkz/jenkins-docker-hub'
      }
    }
    stage('Deploying App to Kubernetes') {
      steps {
        sleep 60
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
          
          
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
