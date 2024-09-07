pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd WebApplication && mvn clean install'
            }
        }
        
        stage('Test') {
            steps {
                sh 'cd WebApplication && mvn test'
            }
        }

        stage('Code Quality Scan') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh "mvn -f WebApplication/pom.xml sonar:sonar"
                }
            }
        }

       # stage('Quality Gate') {
        #    steps {
         #       waitForQualityGate abortPipeline: true
            }
        }

        stage('Push to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'WebApplication', classifier: '', file: 'WebApplicatio/target/WebApplication.war', type: 'war']], credentialsId: 'nexusp', groupId: 'WebApplication', nexusUrl: 'ec2-184-73-112-11.compute-1.amazonaws.com:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0 SNAPSHOT'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tompass', path: '', url: 'http://54.163.141.223:8080/')], 
                contextPath: 'myapp', war: '**/*.war'
            }
        }
    }
}
