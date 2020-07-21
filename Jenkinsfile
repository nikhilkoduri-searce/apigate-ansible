pipeline {

  agent {
      label 'master'
  }


  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('init') {
      steps {
      ansiColor('xterm') {
          ansiblePlaybook(
              playbook: '/var/jenkins_home/workspace/ansible-pipeline/playbook.yml',
              inventory: '/var/jenkins_home/workspace/ansible-pipeline/hosts.ini',
              credentialsId: 'ansible-ssh-key-oslogin',
              colorized: true)
          }
        }
    }


  }

}
