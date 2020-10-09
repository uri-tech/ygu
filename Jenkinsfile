pipeline {
  environment {
    registry = "registry/myweb"
    dockerImage = ""
  }


  agent { label 'kubepod' }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/uri-tech/ygu.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( 'registry:5000' ) {
            dockerImage.push()
            dockerImage.push('latest')
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "kubeconfigId")
        }
      }
    }

  }

}
