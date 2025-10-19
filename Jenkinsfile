pipeline {
  agent any
  tools { jdk 'JDK21'; maven 'Maven3' }   // match names from Manage Jenkins → Tools
  options { timestamps() }
  stages {
    stage('Checkout'){ steps { checkout scm } }
    stage('Build (fat JAR)'){ steps { sh 'mvn -B -DskipTests package spring-boot:repackage' } }
    stage('Test'){ steps { sh 'mvn -B test' }
      post { always { junit 'target/surefire-reports/*.xml' } } }
    stage('Archive'){ steps { archiveArtifacts artifacts: 'target/*.jar', fingerprint: true } }
  }
}
