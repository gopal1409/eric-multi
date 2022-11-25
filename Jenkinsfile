pipeline {
    agent any 
    
    stages {
        stage('code from scm') {
            steps {
                git credentialsId: 'git-pvtkey', url: 'git@github.com:gopal1409/eric-chat-app.git'
            }
        }
        stage('mvn compile') {
            steps {
                sh "mvn -B -DskipTests clean package"
            }
        }
        stage('unit test') {
            steps {
        
                sh "mvn test"
                junit 'target/surefire-reports/*.xml'
            }
        }
         stage('checkstyle') {
            steps {
        
                sh "mvn checkstyle:checkstyle"
                recordIssues(tools: [checkStyle(pattern: '**/checkstyle-result.xml')])
           
            }
        }
  //      stage('sonar') {
    //        steps {
      //  
          //      sh "mvn clean verify sonar:sonar \
  //-Dsonar.projectKey=chatapp \
  //-Dsonar.host.url=http://34.201.21.203:9000 \
  //-Dsonar.login=sqp_769ca63848d49f62a970d0996c2adba2f42e66e4"
               
           
    //        }
      //  }
        stage('nexus') {
            steps {
        
               nexusArtifactUploader artifacts: [[artifactId: 'websocket-demo', classifier: '', file: 'target/websocket-demo-0.0.1-SNAPSHOT.jar', type: 'jar']], credentialsId: 'nexus-cred', groupId: 'websocket-demo', nexusUrl: '34.201.21.203:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '0.0.1-SNAPSHOT'
           
            }
        }
    }
}
