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
                    def scannerHome = tool 'SonarQube Scanner'

                    withSonarQubeEnv('Sonar Server') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=ms-maven -Dsonar.sources=. -Dsonar.java.binaries=target/classes -Dsonar.exclusions='**/*/test/**/*, **/*/acceptance-test/**/*, **/*.html'"
                    }
                }
            }
        }
        stage('DAST'){
            steps{
        		
        		script{
        		   env.DOCKER = tool "Docker"
        		   env.DOCKER_EXEC = "${DOCKER}/bin/docker"

        	
        		    sh "${DOCKER_EXEC} pull owasp/zap2docker-stable"
                    sh '${DOCKER_EXEC} rm -f zap2'
                    sh '${DOCKER_EXEC} run --add-host="localhost:0.0.0.0" --rm -e LC_ALL=C.UTF-8 -e LANG=C.UTF-8 --name zap2 -u zap -p 8090:8090 -d owasp/zap2docker-stable zap.sh -daemon -port 8090 -host 0.0.0.0 -config api.disablekey=true'
                    sh '${DOCKER_EXEC} run --add-host="localhost:0.0.0.0" -v $(pwd):/zap/wrk/:rw --rm -i owasp/zap2docker-stable zap-baseline.py -t "http://zero.webappsecurity.com" -I -r zap_baseline_report.html -l PASS'	
        		   
        		   publishHTML([
        				    allowMissing: false,
        				    alwaysLinkToLastBuild: false,
        				    keepAll: false,
        				    reportDir: '/Users/ojgonzalez/Desktop/report',
        				    reportFiles: 'zap_baseline_report.html',
        				    reportName: 'HTML Report',
        				    reportTitles: ''])
        		}
            }
        }
    }
}
