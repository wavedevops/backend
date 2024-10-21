pipeline {
    agent {
        label 'workstation'
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }
        environment {
            packageJson = ''
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('env variables'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    def appVersion = packageJson.version
                    echo "app version = ${appVersion}"
                }
            }
        }
        stage('test line') {
            steps {
                echo "app version = ${packageJson}"
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
