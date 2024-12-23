pipeline {
    agent any

    stages {
        // Stage for running tests
        stage('Test') {
            steps {
                // Exécuter les tests avec Gradle
                bat './gradlew test'

                // Publier les résultats des tests JUnit

               junit 'build/test-results/test'


                // Publier les résultats des tests Cucumber (facultatif)
                cucumber buildStatus: 'UNSTABLE', reportTitle: 'Cucumber Test Report', fileIncludePattern: 'build/reports/cucumber/*.json'
            }
        }


        // Stage for code analysis using SonarQube


        // Stage for building the project
        stage('Build') {
            steps {
                // Run the build command
                bat './gradlew build'

                // Archive the JAR files generated during the build
                archiveArtifacts artifacts: 'build/libs/*.jar', allowEmptyArchive: true

                // Archive Javadoc files if they are generated
                archiveArtifacts artifacts: '**/build/docs/javadoc/**/*', allowEmptyArchive: true

                // Notify external service (Slack, etc.) about the build status
                notifyEvents message: '<h1>Building...</h1>', token: '4iwq3njk9vw0ui7irxh0yiqhed0rf2qb'
            }

            post {
                // Notifications upon successful build
                success {
                  //  notifyEvents message: '<h1>succeeded and built...</h1>', token: '4lwq3njk9vw0ui7irxh0yiqhed0rf2qb'
                    // Send email notification on success
                    emailext(
                        subject: 'Build Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}',
                        body: 'The build succeeded. Check the details at ${env.BUILD_URL}.',
                        recipientProviders: [[$class: 'DevelopersRecipientProvider']]
                    )
                }

                // Notifications if the build fails
                failure {
                   // notifyEvents message: '<h1>failed to build...</h1>', token: '4iwq3njk9vw0ui7irxh0yiqhed0rf2qb'
                    // Send email notification on failure
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



