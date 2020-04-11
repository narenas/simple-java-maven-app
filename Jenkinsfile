pipeline {
    agent {
        docker { image 'maven:3.3.9-jdk-8-alpine' }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh '''
                    NAME=`mvn help:evaluate -Dexpression=project.name | grep "^[^\[]"` 
                    VERSION=`mvn help:evaluate -Dexpression=project.version | grep "^[^\[]"`
                    ls -l target/${NAME}-${VERSION}.jar
                '''
            }
        }
    }
}

