pipeline {
    agent {
        label 'workstation'
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
    }

    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir() 
        }
        success { 
            echo 'I will run when pipeline is success'
        }
        failure { 
            echo 'I will run when pipeline is failure'
        }
    }
}
