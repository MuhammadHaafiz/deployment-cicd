stage('Checkout') {
    steps {
        git branch: 'main',
            credentialsId: 'github-pat',
            url: 'https://github.com/MuhammadHaafiz/deployment-cicd.git'
    }
}

        stage('Deploy Config') {
            steps {
                sh 'ansible-playbook -i inventory playbook.yml'
            }
        }
    }
}
