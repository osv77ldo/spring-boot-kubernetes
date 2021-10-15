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

        stage('SCA') {
            steps {
                sh 'mvn org.owasp:dependency-check-maven:check'

                archiveArtifacts artifacts: 'target/dependency-check-report.html', followSymlinks: false
            }
        }

        stage('Sonarqube') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner 4.0';

                    withSonarQubeEnv('Sonar Server') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=ms-maven -Dsonar.sources=. -Dsonar.java.binaries=target/classes -Dsonar.exclusions='**/*/test/**/*, **/*/acceptance-test/**/*, **/*.html'"
                    }
                }
            }
        }
    }
}
