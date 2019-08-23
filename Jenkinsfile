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
              }
      stage ('Publish'){
def server = Artifactory.server 'JFrog'
def uploadSpec = """{
"files": [
{
"pattern": "target/hello-0.0.1.war",
"target": "CIUsingSQJfrogGitHub/${BUILD_NUMBER}/",
"props": "Integration-Tested=Yes;Performance-Tested=No"
}
]
}"""
server.upload(uploadSpec)
}
    }
  }
}
