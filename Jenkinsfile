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
          sh "docker login -u ${docker_username} -p ${docker_pass} \
		&& docker push heartocean/cnx-test-repo-1:image${BUILD_NUMBER}"
	}
      }
    }
    stage('modify_compose_file') {
      steps {
        sh "sed -i '3s/Version/image${BUILD_NUMBER}/' docker-compose.yml"
      }
    }

    stage('deploy_app') {
      steps {
	withCredentials([sshUserPrivateKey(credentialsId: 'ssh_auth', keyFileVariable: 'ssh_pvt_key', usernameVariable: 'ssh_username')]) {
          sh "scp -o StrictHostKeyChecking=false -i $ssh_pvt_key docker-compose.yml $ssh_username@100.25.14.244:/home/ubuntu && ssh -i $ssh_pvt_key $ssh_username@100.25.14.244 docker-compose up -d"
        }
      }
    }
  }
}
