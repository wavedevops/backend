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
        appVersion = packageJson.version
        appVersion = '' // variable declaration
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('variables'){
            steps{
                script{
                    
                    echo "app version = ${appVersion}"
                }
            }
        }
        stage('test line') {
            steps {
                echo "app version = $appVersion"
            }
        }
        stage('build'){
            steps {
                sh """
                zip -r -q backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
                """
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
