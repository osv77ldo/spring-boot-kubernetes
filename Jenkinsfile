pipeline {
    agent any

    tools {
        maven 'Maven'
        nodejs 'NodeJs'
    }

    stages {
        stage('initial') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    javac -version
                '''
            }
        }

        stage('compile') {
            tools {
                jdk 'Java'
            }
                steps {
                    sh 'java -version'
                    sh 'mvn -B package --file pom.xml'
                }
        }

        stage('test') {
                steps {
                    sh 'mvn test'
                }
        }
    }
}
