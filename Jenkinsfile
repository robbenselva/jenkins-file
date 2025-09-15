node {
    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        // Run script without needing chmod +x
        sh 'bash hello.sh'
    }

    stage('Archive Output') {
        archiveArtifacts artifacts: 'output.txt', fingerprint: true
    }

    stage('Email Notification') {
        emailext (
            subject: "Jenkins Build #${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
            body: """<p>Hello,</p>
                     <p>The Jenkins job <b>${env.JOB_NAME}</b> build #${env.BUILD_NUMBER} has finished with status: <b>${currentBuild.currentResult}</b>.</p>
                     <p>Output file is available here: 
                        <a href="${env.BUILD_URL}artifact/output.txt">Download</a>
                     </p>
                     <p>Regards,<br/>Jenkins</p>""",
            to: "robbenselva@gmail.com"
        )
    }
}
