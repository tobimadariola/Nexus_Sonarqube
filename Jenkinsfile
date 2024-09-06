pipeline {
    agent any

    stages {   
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
              nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexusp', groupId: 'SampleWebApp', nexusUrl: 'ec2-184-73-112-11.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshot', version: '1.0 SNAPSHOT'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
              deploy adapters: [tomcat9(credentialsId: 'tompass', path: '', url: 'http://54.163.141.223:8080/')], contextPath: 'myapp', war: '**/*.war'
             
              
              
              
          }
            
        }
            
        }
} 
