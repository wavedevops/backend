pipeline {
    agent {
        label 'workstation' // Use the appropriate label for your node
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }

    environment {
        appVersion = '' 
        nexusUrl = 'nexus.chaitu.net/repository/backend/' // Fixed URL
        component = 'backend'
    }

    stages {
        stage('Fetch Package Version from package.json') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version // Assign version to environment variable
                }
                echo "Loaded app version: ${appVersion}"
            }
        }

        stage('Install NPM Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build and Package Application') {
            steps {
                sh """
                zip -r -q ${component}-${appVersion}.zip * -x Jenkinsfile -x ${component}-${appVersion}.zip
                ls -ltr
                """
            }
        }

        stage('Upload Artifact to Nexus Repository') {
            steps {
                script {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http', // Ensure correct protocol
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: "${component}",
                        credentialsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: "${component}",
                            classifier: '',
                            file: "${component}-${appVersion}.zip",
                            type: 'zip']
                        ]
                    )
                }
            }
        }

        stage('Trigger Downstream Deployment Job') {
            steps {
                script {
                    def params = [
                        string(name: 'appVersion', value: "${appVersion}")
                    ]
                    // Current job is the upstream job, triggering 'backend-deploy' as the downstream job
                    build job: 'backend-deploy', parameters: params, wait: false
                }
            }
        }
    }

    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir() // Clean up the workspace
        }
        success { 
            echo 'Pipeline executed successfully!'
        }
        failure { 
            echo 'Pipeline failed!'
        }
    }
}
