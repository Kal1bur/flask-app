pipeline {
  agent {
    node {
      label 'docker'
    }

  }
  stages {
    stage('Git') {
      steps {
        git(url: 'https://github.com/Kal1bur/flask-app.git', branch: 'main')
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t kal1bur/flask-app'
      }
    }

    stage('Docker login') {
      steps {
        sh 'docker login kal1bur dckr_pat_b_H1GuZqdgNbWIPVBOijmceiirc'
      }
    }

    stage('Docker Push') {
      steps {
        sh 'docker push kal1bur/flask-app'
      }
    }

  }
}