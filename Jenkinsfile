pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }

    options { timestamps() }

    stages {

        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build fat JAR') {
            steps { sh 'mvn -B -DskipTests package spring-boot:repackage' }
            post { success { echo 'Fat JAR created in target/' } }
        }

        stage('Test') {
            steps { sh 'mvn -B test' }
            post {
                always {
                    junit testResults: 'target/surefire-reports/*.xml', allowEmptyResults: true
                }
            }
        }

        stage('Archive') {
            steps { archiveArtifacts artifacts: 'target/*.jar', fingerprint: true }
        }

        stage('Deploy to Nexus') {
            steps {
                sh """
                    mvn -B deploy \
                    -DskipTests \
                    -Dnexus.user=admin \
                    -Dnexus.password=Zutkom03\!1107
                """
            }
        }
    }

    post {
        success { echo 'Pipeline finished successfully.' }
        failure { echo 'Pipeline failed.' }
    }
}
