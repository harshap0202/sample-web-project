pipeline {
    agent any

    environment {
        registry = 'docker.io'  
        registryCredential = 'personal-docker' 
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