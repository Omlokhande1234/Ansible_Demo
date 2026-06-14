pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['DEPLOY', 'DESTROY'],
            description: 'Choose operation'
        )
    }

    stages {

        stage('Approval') {
            steps {
                input(
                    message: "Approve ${params.ACTION}?",
                    ok: "Proceed"
                )
            }
        }

        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy') {
            when {
                expression {
                    params.ACTION == 'DEPLOY'
                }
            }

            steps {
                sh '''
                ansible-playbook \
                playbooks/mysql.yml \
                --vault-password-file ~/.vault_pass
                '''
            }
        }

        stage('Destroy') {
            when {
                expression {
                    params.ACTION == 'DESTROY'
                }
            }

            steps {
                sh '''
                ansible-playbook \
                playbooks/destroy.yml \
                --vault-password-file ~/.vault_pass
                '''
            }
        }
    }
}
