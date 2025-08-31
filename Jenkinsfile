pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            credentialsId: 'github-pat',
            url: 'https://github.com/MuhammadHaafiz/deployment-cicd.git'
      }
    }

    stage('Setup Ansible deps') {
      steps {
        sh '''
          set -e
          export PATH="$HOME/.local/bin:$PATH"
          python3 -m pip install --user --upgrade "ansible>=9.0.0" paramiko
          ansible-galaxy collection install community.routeros
        '''
      }
    }

    stage('Deploy Config') {
      steps {
        sh '''
          export PATH="$HOME/.local/bin:$PATH"
          ansible-playbook -i inventory playbook.yml -vv
        '''
      }
    }
  }
}

