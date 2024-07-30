pipeline {
    agent any

    environment {
        registry = 'docker.io'  
        registryCredential = 'personal-docker'
        VM_SUDO_PASS = credentials('vm_pass')
    }

    stages {
        stage ('Checkout') {
            steps {
                git url: 'https://github.com/harshap0202/sample-web-project.git', credentialsId: 'personal-git', branch: 'test'
            }
        }

        stage ('Docker Build') {
            steps {
                script {
                    docker.withRegistry('', registryCredential){
                        def customImage = docker.build("harshpatil02/sample-web-project:${env.BUILD_ID}")
                        customImage.push()
                    }
                }
            }
        }

        stage('Run Playbook') {
            steps{
               sh """
                   export VM_SUDO_PASS=${VM_SUDO_PASS}
                   ansible-playbook playbook.yml -i inventory -u jenkins --extra-vars "ansible_become_pass=${VM_SUDO_PASS}"
                """
            }
        }

    }
    post {
        always {
            echo 'Pipeline Finished'
        }
        success {
            echo 'Status : Successfull'
        }
        failure {
            echo 'Status : Failed'
        }
    }
}