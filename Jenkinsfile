pipeline {
  agent {
      label 'Slave'
  }

   tools {
      maven "Maven_3.6.3"
   }

   environment {
       IMAGE = readMavenPom().getArtifactId()
       VERSION = readMavenPom().getVersion()
   }

   stages {
      stage('Clean up') {
          steps {
              sh "docker rm -f pandaapp || true"
          }
      }
      stage('Get code from repo') {
         steps {
           checkout scm
         }
      }
      stage('Build app') {
          steps {
              sh "mvn clean install -Dmaven.test.skip=true"
          }
      }
      stage('Test app') {
          steps {
              sh "mvn test"
          }
      }
      stage('Docker package') {
          steps {
              sh "mvn package -Pdocker"
          } 
      }
      stage('Run app container') {
          steps {
              sh "docker run -d -p 0.0.0.0:8080:8080 --name pandaapp -t ${IMAGE}:${VERSION}"
          }
      }
   }

}
