pipeline {
    agent {
        label 'workstation'
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }

    stages {
        stage('test') {
            steps{
                sh '''
                echo "this is testing"
                ls  -ltr
                '''
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed. Deleting logs...'
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
