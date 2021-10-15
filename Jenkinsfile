pipeline {
    agent any

    tools {
        maven 'Maven'
        nodejs 'NodeJs'
    }

    stages {
        stage('initial'){
            steps{
             sh '''
              echo "PATH = ${PATH}"
              echo "M2_HOME = ${M2_HOME}"
              java -version
              '''
            }
        }

        stage('compile'){
            agent {
            docker { image 'openjdk:8-jdk-alpine' }
            }
        steps {
            mvn -B package --file pom.xml
        }
        }
    }
}
