pipeline {
    agent any

    environment {
        RECIPIENT = 'robbenselva@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Make Script Executable') {
            steps {
                sh 'chmod +x hello.sh'
            }
        }

        stage('Run Script') {
            steps {
                sh './hello.sh'
            }
        }
    }

    post {
        success {
            emailext(
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Good news!\n\nJob ${env.JOB_NAME} #${env.BUILD_NUMBER} succeeded.\n\nCheck it here: ${env.BUILD_URL}",
                to: "${RECIPIENT}"
            )
        }
        failure {
            emailext(
                subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "Oops!\n\nJob ${env.JOB_NAME} #${env.BUILD_NUMBER} failed.\n\nCheck logs here: ${env.BUILD_URL}",
                to: "${RECIPIENT}"
            )
        }
    }
}
