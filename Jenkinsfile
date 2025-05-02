pipeline {
  ssh_private_key_path= "/var/lib/jenkins/key1.pem"
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
        git credentialsId: 'git_cred', url: 'https://github.com/weirdblackant/maven-web-application.git'
      }
    }
    stage('make_pkg') {
      steps {
        sh 'mvn clean validate && mvn clean package'
      }
    }
    stage('build_image') {
      steps {
        sh "docker build -t heartocean/cnx-test-repo-1:image${BUILD_NUMBER} ."
      }
    }
    stage('push_image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'docker_pass', usernameVariable: 'docker_username')]) {
          sh "docker login -u $docker_username -p ${docker_pass} \
		&& docker push heartocean/cnx-test-repo-1:image${BUILD_NUMBER}"
	}
      }
    }
    stage('deploy_app') {
      steps {
	withCredentials([sshUserPrivateKey(credentialsId: 'ssh_auth', keyFileVariable: 'ssh_private_key_path', usernameVariable: 'ssh_username')]) {
          sh "scp docker-compose.yml -o StrictHostKeyChecking=false -i ${ssh_private_key_path} $ssh_username@100.26.230.156:/home/ubuntu"
        }
      }
    }
  }
}
