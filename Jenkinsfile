@Library('sysadminlib')_


pipeline {
    agent {
        label 'java-agent-01'
    }

    tools {
        maven 'maven-352'
        jdk 'java-11'
    }

    stages {
        stage('Build App') {
            steps {
                sh 'java --version'
                sh 'mvn package install'
            }
        }

        stage('Archive App') {
            steps {
                archiveArtifacts artifacts: '**/*.jar', followSymlinks: false
            }
        }

        stage('Build Docker Image') {
            steps {
                script{
                    def dockerClass = new edu.iti.docker()
                    dockerClass.build("mennayasser5/sysadmin-java", "v${BUILD_NUMBER}")
                }
            }
        }


         stage('push Docker Image') {
            steps {
                script{
                    def dockerClass = new edu.iti.docker()
                    withCredentials([string(credentialsId: 'docker-username', variable: 'DOCKER_USERNAME'), string(credentialsId: 'docker-username', variable: 'DOCKER_PASSWORD')]) {
                        dockerClass.login("${DOCKER_USERNAME}", "${DOCKER_PASSWORD}")
                    }
                    dockerClass.push("mennayasser5/sysadmin-java", "v${BUILD_NUMBER}")
                }
            }
        }

        
    }
}
