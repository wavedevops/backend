pipeline {
    agent {
        label 'workstation'
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }

    stages {
        stage('install dependencies') {
            steps {
                sh '''
                    npm install
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed. Deleting logs...'
            script {
                deleteLogs()
            }
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}

// Helper method to delete build logs
// void deleteLogs() {
//     def logFile = new File("${currentBuild.rawBuild.getLogFile()}")
//     if (logFile.exists()) {
//         logFile.delete()
//         echo 'Build logs deleted successfully.'
//     } else {
//         echo 'No logs found to delete.'
//     }
// }
