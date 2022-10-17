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
               nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'sleep', groupId: 'SampleWebApp', nexusUrl: '3.236.218.232:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.2-SNAPSHOT'
                
            }
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tomcat1', path: '', url: '54.84.3.165:8080')], contextPath: 'webapp', war: '**/*.war'
              
          }
            
        }

            
        }
}  
 
