pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                // Exécuter les tests avec Gradle
                bat './gradlew test'

                // Archiver les résultats des tests unitaires
                junit '**/build/test-results/test/*.xml'

                // Générer et archiver les rapports de tests Cucumber
                cucumber build: "build/reports/cucumber/", fileIncludePattern: '**/*.json', sortingMethod: "ALPHABETICAL"
            }
        }

        stage('Build') {
            steps {
                bat './gradlew build'

                // Archiver les artefacts générés
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
