pipeline {
  agent {
        label "docker-slave"
    }
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') {
      steps {
        sleep 300
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
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
          sleep 30
          
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
