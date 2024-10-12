pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/aynuod/JPA-Jenkins.git', branch: 'main'
            }
        }

        // Étape pour l'analyse SonarQube sans les fichiers .java
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') {
                        bat """
                            ${scannerHome}\\bin\\sonar-scanner.bat ^
                            -Dsonar.projectKey=sonar-xakra ^
                            -Dsonar.host.url=http://localhost:9000 ^
                            -Dsonar.login=squ_7e47860cbf91e8e814283d6ae99dfa93920f3c45 ^
                            -Dsonar.sources=./src ^
                            -Dsonar.java.binaries=./target/classes
                            -X
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
