pipeline {
  environment {
    registry = 'kal1bur/flask_app'
    registryCredentials = 'docker'
    cluster_name = 'skillstorm'
    namespace = 'kal1bur'
  }
  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/Kal1bur/flasky.git', branch: 'main')
      }
    }

    stage('Build Stage') {
      steps {
        script {
          dockerImage = docker.build(registry)
        }
      }
    }

    stage('Deploy Stage') {
      steps {
        script {
          docker.withRegistry('', registryCredentials) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Kubernetes') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsID: 'AWS_SECRET_ACCESS_KEY')]) {
          sh "aws eks update-kubeconfig --region us-east-1 --name ${cluster_name}"
          script{
            try{
              sh "kubectl create namespace ${namespace}"
            } catch (Exception e) {
              echo "Exception handled"
            }
          } 
          sh "kubectl apply -f deployment.yaml -n ${namespace}"
          sh "kubectl -n ${namespace} rollout restart deployment flaskcontainer"
        }
      }
    }

    // stage('Build') {
    //   steps {
    //     sh 'docker build -t kal1bur/flask_app .'
    //   }
    // }

    // stage('Docker login') {
    //   steps {
    //     sh 'docker login -u kal1bur -p dckr_pat_b_H1GuZqdgNbWIPVBOijmceiirc'
    //   }
    // }

    // stage('Docker Push') {
    //   steps {
    //     sh 'docker push kal1bur/flask-app'
    //   }
    // }

  }
}
