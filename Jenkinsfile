pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/EnjyHenry/TMDB-API-Testing-Postman-Newman.git'
            }
        }

        stage('Verify Newman') {
            steps {
                sh 'newman --version'
            }
        }

        stage('Run Postman Tests') {
            steps {
                withCredentials([
                    file(
                        credentialsId: 'a81ab8e1-d5be-426a-9efd-806caa3cf290',
                        variable: 'ENV_FILE'
                    )
                ]) {
                    sh '''
                        mkdir -p reports

                        newman run "TMDB.postman_collection.json" \
                            -e "$ENV_FILE" \
                            -r cli,htmlextra \
                            --reporter-htmlextra-export reports/report.html \
                            --reporter-htmlextra-skipHeaders "Authorization"
                    '''
                }
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'reports',
                reportFiles: 'report.html',
                reportName: 'Newman HTML Report'
            ])

            archiveArtifacts(
                artifacts: 'reports/report.html',
                allowEmptyArchive: true
            )
        }
    }
}
