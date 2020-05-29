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
           git credentialsId: 'f9b04898-9e16-402c-a4f5-e41f60e88d00', url: 'https://github.com/arturkwietniak/apka_maven.git'
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
      stage('Selenium Tests') {
          steps {
              sh "mvn test -Pselenium"
          }
      }

   }

}
