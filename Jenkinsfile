pipeline {
    agent any

    environment {
        // Name of SonarQube server configured in Jenkins -> Manage Jenkins -> System
        SONARQUBE = 'SonarQube_server'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Vamshi420/maven_sonar.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('SonarQube Code Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=devops-app \
                        -Dsonar.projectName=devops-app \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage("Quality Gate") {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
