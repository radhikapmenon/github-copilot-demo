pipeline {
  agent any

  tools {
    maven 'mvn'
  }

  stages {
    stage('Build App') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('Build Image') {
      steps {
      sh "docker build -t radhikapmenon/github-copilot-demo:${env.BUILD_ID} ."
      sh "docker tag radhikapmenon/github-copilot-demo:${env.BUILD_ID} radhikapmenon/github-copilot-demo:latest"
      }
    }
    
  }

  post {
    always {
      archive 'target/**/*.jar'
    }
    success {
      withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
        sh "docker push radhikapmenon/github-copilot-demo:${env.BUILD_ID}"
        sh "docker push radhikapmenon/github-copilot-demo:latest"
      }
    }
  }
}
