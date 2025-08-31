pipeline {
  agent any
  environment {
    PATH = "${env.HOME}/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
    ANSIBLE_COLLECTIONS_PATHS = "/usr/share/ansible/collections:${env.HOME}/.ansible/collections"
    ANSIBLE_PERSISTENT_CONTROL_PATH_DIR = "${env.HOME}/.ansible/pc"
    ANSIBLE_HOST_KEY_CHECKING = "False"
  }
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
          # Buat folder untuk control socket agar pasti ada
          mkdir -p "$HOME/.ansible/pc"

          # Pasang Ansible & libs untuk koneksi network_cli
          python3 -m pip install --user --upgrade "ansible>=9.0.0" paramiko ansible-pylibssh

          # Pastikan koleksi tersedia untuk user jenkins
          ansible-galaxy collection install community.routeros ansible.netcommon -p "/usr/share/ansible/collections" --force

          echo "== Sanity check =="
          ansible --version || true
          ansible-config dump | grep -E "COLLECTIONS_PATHS|PERSISTENT_CONTROL_PATH_DIR" || true
          ls -ld "$HOME/.ansible/pc" || true
        '''
      }
    }

    stage('Deploy Config') {
      steps {
        sh '''
          ansible-playbook -i inventory playbook.yml -vv
        '''
      }
    }
  }
}

