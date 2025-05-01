pipeline {
  agent any
  tools {
    git 'Default'
    maven 'maven1'
  }
  triggers {
    githubPush()
  }
  stages {
    stage('git_scm') {
      steps {
	git credentialsId: 'github_cred', url: 'https://github.com/weirdblackant/maven-web-application.git'
      }
    }
    stage('make_pkg') {
      steps {
        sh 'mvn clean validate && mvn clean package'
      }
    }
  }
}
