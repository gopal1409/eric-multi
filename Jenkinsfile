pipeline {
    environment {
        registry = '44.205.13.75:8085/chatapp'
        registryCredential = 'nexus-cred'
        dockerImage= ''
        tag = sh(returnStdout: true, script: "git rev-parse --short=10 HEAD").trim()
    }
    agent any 
    stages {
        stage ('scm') {
            steps {
                git credentialsId: 'git-pvtkey', url: 'git@github.com:gopal1409/eric-chat-app.git'
            }
        }
        stage ('build') {
            steps {
               sh 'mvn clean package'
            }
        }
        stage ('build image') {
            steps {
               script {
                   dockerImage=docker.build registry + "$tag"
               }
            }
        }
        stage ('push image') {
            steps {
               script {
                   // This step should not normally be used in your script. Consult the inline help for details.
                      docker.withRegistry('http://44.205.13.75:8085',registryCredential) {
                       dockerImage.push()
                     }
               }
            }
        }
    }
}
