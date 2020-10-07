pipeline {

  environment {
    registry = "registry:5000/registry/myweb"
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
          dockerImage = registry.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          registry.withRegistry( "" ) {
            dockerImage.push()
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
