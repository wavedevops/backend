pipeline {
    agent {
        label 'workstation' // Use the appropriate label for your node
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }

    environment {
        // Placeholder - environment variables cannot use 'readJSON' directly.
        appVersion = '' 
    }

    stages {
        stage('Load Package Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version // Assign version to environment variable
                }
                echo "Loaded app version: $appVersion"
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test Line') {
            steps {
                echo "App version = ${appVersion}"
            }
        }

        stage('Build') {
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
            deleteDir() // Clean up the workspace
        }
        success { 
            echo 'I will run when pipeline is successful'
        }
        failure { 
            echo 'I will run when the pipeline fails'
        }
    }
}
