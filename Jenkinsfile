pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                // Execute tests with Gradle
                bat './gradlew test'

                // Publish JUnit test results

                junit '**/build/test-results/test/*.xml'
                                 cucumber buildStatus: 'UNSTABLE', reportTitle: 'Cucumber Test Report', fileIncludePattern: 'build/reports/cucumber/*.json'

                // Publish Cucumber test results (if applicable)
                }
        }

        stage('Build') {
            steps {
                bat './gradlew build'
                archiveArtifacts artifacts: 'build/libs/*.jar', allowEmptyArchive: true
                archiveArtifacts artifacts: '**/build/docs/javadoc/**/*', allowEmptyArchive: true
                  }

            post {


                success {
                    emailext(
                        subject: 'Build Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}',
                        body: 'The build succeeded. Check the details at ${env.BUILD_URL}.',
                        recipientProviders: [[$class: 'DevelopersRecipientProvider']]
                    )
                }
                failure {
                    emailext(
                        subject: 'Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}',
                        body: 'The build failed. Check the details at ${env.BUILD_URL}.',
                        recipientProviders: [[$class: 'DevelopersRecipientProvider']]
                    )
                }
            }
        }
    }
}
