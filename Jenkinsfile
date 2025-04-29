pipeline {
  agent any
  tools {
    git 'Default'
    maven 'maven1'
  }
  stages {
    stage('git_scm') {
      steps {
        git credentialsId: 'git_token', url: 'https://github.com/weirdblackant/maven-web-application.git'
      }
    }
  }
}
