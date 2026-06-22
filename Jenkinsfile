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

    stage('Git Checkout') {
        steps {
            checkout scm
        }
    }
    stage('Approval') { 
        steps { 
            script { 
                input( 
                    message: "ACTION selected: ${params.ACTION}. Do you want to continue?", 
                    ok: "Proceed" 
                ) 
            } 
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
            -i inventory/hosts \
            playbooks/mysql.yml \
            --private-key /var/lib/jenkins/.ssh/config.pem \
            --vault-password-file /home/ec2-user/Ansible_Demo/vault_pass.txt
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
            -i inventory/hosts \
            playbooks/destroy.yml \
            --private-key /var/lib/jenkins/.ssh/config.pem \
            --vault-password-file /home/ec2-user/Ansible_Demo/vault_pass.txt
            '''
        }
    }
}

post {
    success {
        echo 'Pipeline completed successfully.'
    }

    failure {
        echo 'Pipeline failed. Check console logs.'
    }
}

}


