pipeline {
    agent any

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs() // Clean workspace at the start of the pipeline
            }
        }

        stage('Test') {
            steps {
                bat './gradlew test'
                junit 'build/test-results/test/*.xml'
            }
        }

        stage('Build') {
            steps {
                bat './gradlew build'
                archiveArtifacts artifacts: 'build/libs/*.jar', allowEmptyArchive: true
            }
        }
    }
}
