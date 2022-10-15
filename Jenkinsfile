pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                git 'https://github.com/PRODOGRACE/sonarqube-nexusRepo.git'
            }
        }
        
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
                 steps {
                   sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {
            steps {
                  withSonarQubeEnv('sonar-server') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp /target/SampleWebApp.war', type: 'war']], credentialsId: 'NEXUS', groupId: 'SampleWebApp', nexusUrl: 'http://3.233.229.41:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-snapshot'
                
            }
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://34.201.108.176:8080/')], contextPath: 'myapp', war: '**/*.war'
              
          }
            
        }

            
        }
}  
 
