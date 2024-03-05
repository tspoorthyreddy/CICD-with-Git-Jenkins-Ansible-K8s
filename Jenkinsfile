pipeline {
    agent any
    stages {
        stage('Fetch Code') {
            steps {
                git branch: 'CICD-using-Git,-Jenkins-&-Maven', url: 'https://github.com/tspoorthyreddy/CICD-with-Git-Jenkins-Ansible-K8s.git'
            }
        }
        stage('Build') {
            steps {
               sh 'mvn clean package' 
            }
        }
        stage('Test') {
            steps {
               sh 'mvn test' 
            }
        }
        stage('Static Code Analysis') {
            environment {
                SONAR_URL = "http://3.85.61.93:9000"
            }
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_AUTH_TOKEN')]) {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
                }
            }
        }
        stage('Copy Artifact to Ansible') {
            steps {
               sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//docker', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)]) 
            }
        }
    }
}
