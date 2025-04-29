pipeline {
  tools {
    git 'default'
    maven 'maven1'
  }
  stages {
    stage('git_scm') {
      steps {
        git 'https://github.com/weirdblackant/maven-web-application.git',branch: 'master'
      }
    }
  }
}
