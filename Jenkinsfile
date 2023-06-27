pipeline {
  agent any
  stages {
      stage('Build Artifact') {
          steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar'  //test
            }
        }   
     stage('Unit Tests - JUnit and JaCoCo') {
         steps {
           sh "mvn test"
         }
          post {
            always {
              junit 'target/surefire-reports/*.xml'
              jacoco execPattern: 'target/jacoco.exec'
            }
          }
      }
      stage('Docker Build and Push') {
        steps {
          withCredentials([string(credentialsId: 'dockerhub-password-andrea', variable: 'DOCKER_HUB_PASSWORD')]) {
              sh 'sudo docker login -u andrea43700 -p $DOCKER_HUB_PASSWORD'
              sh 'printenv'
              sh 'sudo docker build -t andrea43700/app-test-jenkins:""$GIT_COMMIT"" .'
              sh 'sudo docker push andrea43700/app-test-jenkins:""$GIT_COMMIT""'
          }
        }
      }
    }    
}
