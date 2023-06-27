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
          withDockerRegistry(credentialsId: 'dockerhub-andrea', url: '') {
            sh 'printenv'
            sh 'sudo docker build -t andrea43700/numeric-app:""$GIT_COMMIT"" .'
            sh 'docker push andrea43700/numeric-app:""$GIT_COMMIT""'
          }
        }
      }
    }    
}
