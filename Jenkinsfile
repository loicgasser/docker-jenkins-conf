pipeline {

  agent {
    label 'docker'
  }

  stages {

    stage('build docker image') {

      steps {
        sh "git describe --abbrev=0 --tags > .git/last-tag"
        def last_tag = readFile('.git/last-tag').trim()
        println last_tag
        docker.build("camptocamp/jenkins-conf:${last_tag}")
      }

    }

  }

}