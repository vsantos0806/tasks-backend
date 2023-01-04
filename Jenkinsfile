pipeline {
    agent any 
    stages {
        stage ('Build backend') {
            steps {
                bat 'mvn clean package -DskipTests=true'
            } 
        }
        stage ('Unit Tests') {
            steps {
                bat 'mvn test'
            } 
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            } 
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {                
                    bat "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=f59dc5a00f2013b0117c8a5adbd5e9cec51c7aa7 -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/.mvn/**,**/src/test/**,**/model/**,**Application.java"                
                }
            }
        }
        stage ('Quality Gate') {
            steps {
                sleep(5)
                timeout(time: 1, unit: 'MINUTES') {                
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }    
}