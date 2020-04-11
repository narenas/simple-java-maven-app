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
            environment {
                NEXUS_CRED = credentials('Qindel-narenas-credentials')
                CONFIG_FIE = configFile('4e515527-68b5-4af1-b63e-eb350d2b8233')
            }
            steps {
                configFileProvider([configFile(fileId: '4e515527-68b5-4af1-b63e-eb350d2b8233', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                    sh '''
                    mvn jar:jar install:install help:evaluate -Dexpression=project.name
                    NAME=`mvn help:evaluate -Dexpression=project.name | grep "^[^\\[]"` 
                    VERSION=`mvn help:evaluate -Dexpression=project.version | grep "^[^\\[]"`
                    mvn -gs $MAVEN_GLOBAL_SETTINGS deploy
                    '''
                }
                // sh '''
                //     mvn jar:jar install:install help:evaluate -Dexpression=project.name
                //     NAME=`mvn help:evaluate -Dexpression=project.name | grep "^[^\\[]"` 
                //     VERSION=`mvn help:evaluate -Dexpression=project.version | grep "^[^\\[]"`
                //     sh 'mvn clean install -Dserver.username=${NEXUS_CRED_USR} -Dserver.password=${NEXUS_CRED_PSW}'
                // '''
                }

            }
        }
    }
}

