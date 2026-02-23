pipeline {
    agent any

    triggers {
        cron('H/5 * * * 1')   // every 5 minutes on Mondays
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build + Test') {
            steps {
                sh 'mvn -B clean test'
            }
        }

        stage('JaCoCo Report') {
            steps {
                sh 'mvn -B jacoco:report'
            }
        }

        stage('Package Artifact') {
            steps {
                sh 'mvn -B package -DskipTests'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            archiveArtifacts artifacts: 'target/site/jacoco/**', fingerprint: true
        }
    }
}
