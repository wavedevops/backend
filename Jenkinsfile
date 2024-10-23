// it is a ci job  upstream job
pipeline {
    agent {
        label 'workstation' // Use the appropriate label for your node
    }

    options {
        ansiColor('xterm')        // Enable ANSI color output
        disableConcurrentBuilds() // Ensure the pipeline runs only once at a time
    }

    environment {
        // Placeholder - environment variables cannot use 'read JSON' directly.
        appVersion = '' 
        nexusUrl = 'nexus.chaitu.net/repository/backend/' // Fixed URL
        component = 'backend'
    }

    stages {
        stage('Load Package Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version // Assign version to environment variable
                }
                echo "Loaded app version: ${appVersion}"
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh """
                zip -r -q ${component}-${appVersion}.zip * -x Jenkinsfile -x ${component}-${appVersion}.zip
                ls -ltr
                """
            }
        }

        stage('Nexus Artifact Upload') {
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
    }
    stage('Deploy'){
        steps{
            script{
                def params = [
                    string(name: 'appVersion', value: "${appVersion}")
                ]
                build job: 'backend-deploy', parameters: params, wait: false
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




// pipeline {
//     agent {
//         label 'workstation'
//     }
//     options {
//         timeout(time: 30, unit: 'MINUTES')
//         disableConcurrentBuilds()
//         ansiColor('xterm')
//     }
//     environment{
//         def appVersion = '' //variable declaration
//         nexusUrl = 'nexus.chaitu.net/repository/backend/'
//     }
//     stages {
//         stage('read the version'){
//             steps{
//                 script{
//                     def packageJson = readJSON file: 'package.json'
//                     appVersion = packageJson.version
//                     echo "application version: $appVersion"
//                 }
//             }
//         }
//         stage('Install Dependencies') {
//             steps {
//                sh """
//                 npm install
//                 ls -ltr
//                 echo "application version: $appVersion"
//                """
//             }
//         }
//         stage('Build'){
//             steps{
//                 sh """
//                 zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
//                 ls -ltr
//                 """
//             }
//         }

//         stage('Nexus Artifact Upload'){
//             steps{
//                 script{
//                     nexusArtifactUploader(
//                         nexusVersion: 'nexus3',
//                         protocol: 'http',
//                         nexusUrl: "${nexusUrl}",
//                         groupId: 'com.expense',
//                         version: "${appVersion}",
//                         repository: "backend",
//                         credentialsId: 'nexus-auth',
//                         artifacts: [
//                             [artifactId: "backend" ,
//                             classifier: '',
//                             file: "backend-" + "${appVersion}" + '.zip',
//                             type: 'zip']
//                         ]
//                     )
//                 }
//             }
//         }
//     //     stage('Deploy'){
//     //         steps{
//     //             script{
//     //                 def params = [
//     //                     string(name: 'appVersion', value: "${appVersion}")
//     //                 ]
//     //                 build job: 'backend-deploy', parameters: params, wait: false
//     //             }
//     //         }
//     //     }
//     }
//     post { 
//         always { 
//             echo 'I will always say Hello again!'
//             deleteDir()
//         }
//         success { 
//             echo 'I will run when pipeline is success'
//         }
//         failure { 
//             echo 'I will run when pipeline is failure'
//         }
//     }
// }