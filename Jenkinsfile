pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17'
        }

      }
      steps {
        echo 'compiling sysfoo app..'
        sh 'mvn compile'
      }
    }

    stage('unit test') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17'
        }

      }
      steps {
        echo 'running unit tests...'
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.9.6-eclipse-temurin-17'
        }

      }
      steps {
        echo 'package the artifact...'
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.jar'
      }
    }

  }
  
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}
