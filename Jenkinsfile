import java.text.SimpleDateFormat

pipeline {
  options {
    buildDiscarder logRotator(numToKeepStr: '5')
    disableConcurrentBuilds()
  }
  agent {
    kubernetes {
      cloud "kubernetes"
      label "prod"
      yamlFile "KubernetesPod.yaml"
    }      
  }
  environment {
    cmAddr = "cm.192.168.12.247.nip.io"
  }
  stages {
    stage("deploy") {
      when {
        branch "master"
      }
      steps {
        container("helm") {
          sh "helm upgrade prod helm --namespace prod"
        }
      }
    }
    stage("test") {
      when {
        branch "master"
      }
      steps {
        echo "Testing..."
      }
      post {
        failure {
          container("helm") {
            sh "helm rollback prod 0"
          }
        }
      }
    }
  }
}
