pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Eclipse Adoptium\\jdk-25.0.5.8-hotspot'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code checkout completed'
            }
        }

        stage('Verify Environment') {
            steps {
                bat '''
                    set "PATH=%JAVA_HOME%\\bin;%PATH%"
                    echo Checking Java version
                    java -version

                    echo Checking Maven Wrapper
                    call mvnw.cmd -version
                '''
            }
        }

        
stage('Test Credentials') {
    steps {
        withCredentials([
            string(
                credentialsId: 'demo-secret',
                variable: 'DEMO_SECRET'
            )
        ]) {
            bat '''
                @echo off

                if defined DEMO_SECRET (
                    echo Demo credential injected successfully.
                ) else (
                    echo Credential injection failed.
                    exit /b 1
                )
            '''
        }
    }
}


        stage('Build and Test') {
            steps {
                bat '''
                    set "PATH=%JAVA_HOME%\\bin;%PATH%"
                    call mvnw.cmd -B -ntp clean verify
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'sb-backend/target/*.jar',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}
