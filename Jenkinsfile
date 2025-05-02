pipeline {
  env.image_name= "heartocean/cnx-test-repo-1:im-${BUILD_NUMBER}"
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
    stage('build_image') {
      steps {
        sh "docker build -t $image_name ."
      }
    }
    stage('push_image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker_pass', passwordVariable: 'docker_pass', usernameVariable: 'docker_username')]) {
          sh "docker login -u ${docker_username} -p ${docker_pass} \
                && docker push $image_name"
        }
      }
    }
    stage('deploy_app') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ssh_private_key_file', keyFileVariable: 'ssh_key', usernameVariable: 'username')]) {
	  sh "ssh -o StrictHostKeyChecking=false ubuntu@100.26.230.156 ls"
        }
      }
    }
  }
}
