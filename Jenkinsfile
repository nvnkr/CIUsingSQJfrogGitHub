pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Build') {
      steps {
        bat 'mvn clean verify -DskipITs=true'
      }
    }
    stage('UnitTesting') {
      steps {
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
      }
    }
    stage('Static_Code_Analysis') {
      steps {
        bat 'mvn clean verify sonar:sonar -Dsonar.projectName=CIUsingSQJfrogGitHub -Dsonar.projectKey=CIUsingSQJfrogGitHub -Dsonar.projectVersion=$BUILD_NUMBER'
      }
    }
    stage('Integration_Test') {
      steps {
        bat 'mvn clean verify -Dsurefire.skip=true'
        junit '**/target/failsafe-reports/TEST-*.xml'
        archiveArtifacts 'target/*.jar'
      }
    }
  }
}