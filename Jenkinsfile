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
      parallel {
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

        stage('Docker BnP') {
          agent any
          steps {
            script {
              docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
                // Fetch the full Git commit hash and truncate it to the first 7 characters
                def commitHash = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
                // Build the Docker image with the short commit hash as tag
                // replace xxxxxx with your docker hub username/org
                def dockerImage = docker.build("initcron/sysfoo:${commitHash}", "./")
                // Push the image tagged with the commit hash
                dockerImage.push()
                // Push additional standard tags if needed
                dockerImage.push("latest")
                dockerImage.push("dev")
              }
            }

          }
        }

      }
    }

  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}