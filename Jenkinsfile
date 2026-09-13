Groovy
pipeline {
    agent any

    tools {
        nodejs 'mynodejs' 
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Automatically pulls the GitHub repository
                checkout scm
            }
        }

        stage('Run Postman Tests') {
            steps {
                bat '''
                    if not exist results mkdir results
                    newman run Products_test.postman_collection.json -d product_data.json -g OnlineStoreAPI.postman_globals.json -r htmlextra --reporter-htmlextra-export ./results/report.html --reporter-htmlextra-browserTitle "Store API Test Report" --reporter-htmlextra-title "Store API Test Summary" --suppress-exit-code
                '''
            }
        }
    }

    post {
        always {
            // Archives the HTML report in Jenkins
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'results',
                reportFiles: 'report.html',
                reportName: 'HTML Extra Test Report',
                reportTitles: 'Store API Test Summary'
            ])
        }
    }
}
