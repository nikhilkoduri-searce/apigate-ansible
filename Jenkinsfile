pipeline {

  agent {
      label 'jenkins-ansible-slave'
  }


  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }


    stage('set ansible role path') {
      steps {
        sh 'export ANSIBLE_ROLES_PATH=$WORKSPACE/roles'
      }
    }


    stage('ansible') {
      steps {
      ansiColor('xterm') {
          ansiblePlaybook(
              playbook: '/var/jenkins_home/workspace/ansible-pipeline/apigate-ansible/playbook.yaml',
              inventory: '/var/jenkins_home/workspace/ansible-pipeline/apigate-ansible/hosts.ini',
              credentialsId: 'ansible-ssh-key-oslogin',
              colorized: true)
          }
        }
    }
  }
}
