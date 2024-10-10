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
                            -Dsonar.projectKey=spring-test ^
                            -Dsonar.host.url=http://localhost:9000 ^
                            -Dsonar.login=sqp_c42854901308a8b594be233bafa1f87abeccb71b ^
                            -Dsonar.sources=./src ^
                            -Dsonar.java.binaries=./target/classes
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
    post {
        success {
            emailext to: 'your.email@example.com',
                     subject: "Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                     body: "The build was successful!\n\nCheck the details here: ${env.BUILD_URL}"
        }
        failure {
            emailext to: 'your.email@example.com',
                     subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                     body: "The build failed.\n\nCheck the details here: ${env.BUILD_URL}"
        }
    }
}
