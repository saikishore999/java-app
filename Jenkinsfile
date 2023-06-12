pipeline{
    agent any
     environment {
        def scannerHome = tool 'sonarqube-scanner'
    }
    stages{
        stage("build"){
            steps{
                sh """
                ls -lrt
                mvn install
                """
            }
        }
        stage("Code Analysis"){
            steps {
                withSonarQubeEnv('sonar-jenkins') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=spring-key -Dsonar.projectName=spring -Dsonar.sources=. -Dsonar.java.binaries=target/classes -Dsonar.sourceEncoding=UTF-8"
                }
            }
        }
         stage("Quality Gate") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}