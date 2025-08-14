pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/<username>/deployment-ci-cd.git'
            }
        }
        stage('Deploy Config') {
            steps {
                sh 'ansible-playbook -i inventory playbook.yml'
            }
        }
    }
}
