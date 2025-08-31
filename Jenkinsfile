pipeline {
  agent any
  environment {
    PATH = "${env.HOME}/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
    # pakai VAR yang benar (singular) + arahkan ke HOME jenkins lebih dulu
    ANSIBLE_COLLECTIONS_PATH = "${env.HOME}/.ansible/collections:/usr/share/ansible/collections"
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
          mkdir -p "$HOME/.ansible/pc" "$HOME/.ansible/collections"

          # Ansible + driver SSH untuk network_cli
          python3 -m pip install --user --upgrade "ansible>=9.0.0" paramiko ansible-pylibssh

          # INSTALL KOLEKSI ke HOME jenkins (bukan /usr/share)
          ansible-galaxy collection install community.routeros ansible.netcommon \
            -p "$HOME/.ansible/collections" --force

          echo "== Sanity =="
          ansible --version || true
          ansible-config dump | grep -E "COLLECTIONS_PATH|PERSISTENT_CONTROL_PATH_DIR" || true
          ls -ld "$HOME/.ansible/pc" || true
          ansible-galaxy collection list | grep -E "routeros|netcommon" || true
          ls -l "$HOME/.ansible/collections/ansible_collections/community/routeros/plugins/modules" | head || true
        '''
      }
    }

    stage('Deploy Config') {
      steps {
        sh 'ansible-playbook -i inventory playbook.yml -vv'
      }
    }
  }
}

