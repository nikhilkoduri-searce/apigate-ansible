pipeline {

  agent {
      label 'jenkins-slave'
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
              playbook: './playbook.yml',
              inventory: './hosts.ini',
              credentialsId: 'ansible-ssh-key-oslogin',
              colorized: true)
          }
        }
    }


  }

}
