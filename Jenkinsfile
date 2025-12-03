pipeline {
    agent {
        label 'java-agent-01'
    }

    tools {
        maven 'maven-352'
        jdk 'java-11'
    }

    stages {
        stage('checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Hassan-Eid-Hassan/cicd-lab2.git'
            }
        }

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
                sh'docker build -t mennayasser5/sysadmin-java:v1 .'
            }
        }

         stage('push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'docker-username', variable: 'DOCKER-USERNAME'), string(credentialsId: 'docker-password', variable: 'DOCKER-PASSWORD')]) {
                sh 'docker login -u ${DOCKER-USERNAME} -p ${DOCKER-PASSWORD}'
                sh'docker push mennayasser5/sysadmin-java:v1'
            }
                sh'docker push mennayasser5/sysadmin-java:v1'
            }
        }
    }
}
