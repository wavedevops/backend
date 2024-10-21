pipeline {
    agent {
        label 'workstation'
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }
        environment {
        def packageJson = readJSON file: 'package.json'
        def appVersion = packageJson.version
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('test line') {
            steps {
                echo "app version = ${appVersion}"
                echo "app version : $appVersion"
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
