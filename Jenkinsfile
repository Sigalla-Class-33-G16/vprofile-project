pipeline {
    agent any

    tools {
        maven 'MAVEN3.9.9'
        jdk 'JDK17'
    }

    environment {
        SNAP_REPO      = 'vprofile-snapshot'
        RELEASE_REPO   = 'vprofile-release'
        CENTRAL_REPO   = 'vpro-maven-central'
        NEXUSIP        = '172.31.2.130'
        NEXUSPORT      = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN    = 'nexuslogin'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }

            post {
                success {
                    echo 'Maven build successful.'
                    echo 'Now Archiving.'

                    archiveArtifacts artifacts: '**/*.war'
                }

                failure {
                    echo 'Maven build failed.'
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }

        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}