pipeline {
  environment {
    registry = "myawspk/myrepo"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Building Docker Image') {
      steps{
      // Get code from a GitHub repository
      git url: 'https://github.com/myawspk/jenkins.git', branch: 'main',
      credentialsId: 'github'
	    script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image To Docker Hub') {
      steps{
        script {
          /* Finally, we'll push the image with two tags:
                   * First, the incremental build number from Jenkins
                   * Second, the 'latest' tag.
                   * Pushing multiple tags is cheap, as all the layers are reused. */
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
              dockerImage.push("${env.BUILD_NUMBER}")
              dockerImage.push("latest")
          }
        }
      }
    }
    stage('Deploy to Kubernetes'){
        steps{
            sh 'whoami'
            sh 'sudo -i'
            sh 'whoami'
            sh 'export KUBECONFIG=/root.kube/config'
            sh 'kubectl apply -f deployment.yml'
       }
    }
  }
  
  }

