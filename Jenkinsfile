pipeline {
    agent any 
    stages {
        stage ('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Bhargav863/java-project-maven.git'
            }
        }
        stage ('compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage ('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('sonarqube Analysis') {
            steps {
                withSonarQubeEnv('Sonarqube') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                }
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage ('upload to nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'myapp', classifier: '', file: 'target/myapp.war', type: '.war']], credentialsId: 'nexus', groupId: 'in.reyaz', nexusUrl: '35.154.201.11:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'hotstar', version: '8.3.3-SNAPSHOT'
            }
        }
        stage ('Deploy to tomcat') {
            steps {
                deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat-cred', path: '', url: 'http://65.0.18.241:8080/')], contextPath: 'myapp', war: '**/*.war'
            }
        }
    }
}
