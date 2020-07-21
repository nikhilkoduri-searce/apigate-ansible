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


    stage('set ansible env') {
      steps {
        sh 'export ANSIBLE_ROLES_PATH=$WORKSPACE/roles'
        sh 'export ANSIBLE_HOST_KEY_CHECKING=False'
      }
    }

    stage('ansible') {
      steps {
      ansiColor('xterm') {
          ansiblePlaybook(
              playbook: '/home/jenkins/agent/workspace/ansible-pipeline/playbook.yaml',
              inventory: '/home/jenkins/agent/workspace/ansible-pipeline/hosts.ini',
              credentialsId: 'ansible-ssh-key-oslogin',
              colorized: true)
          }
        }
    }
  }
}
