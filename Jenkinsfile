pipeline {
    agent any

    tools {
        maven 'Maven'
        node 'NodeJS'
    }

  
    stages {
        stage('initial'){
            steps{
             sh '''
              echo "PATH = ${PATH}"
              echo "M2_HOME = ${M2_HOME}"
              '''
            }
        }       

        stage('compile'){
            steps{
             sh 'mvn clean compile -e'
            }
        }    
}
}
