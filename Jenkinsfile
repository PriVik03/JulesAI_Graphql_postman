pipeline {
    agent any
    stages {
        stage('Checkout Repository') {
            steps {
                script {
                    echo "Checking out repository"
                    git branch: 'main', url: 'https://github.com/PriVik03/JulesAI_Graphql_postman.git'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo "Installing dependencies"
                    // Check if npm is installed
                    bat 'npm --version'  // This will print npm version to confirm it's available
                    // Install Newman globally (optional if using npx)
                    bat 'npm install -g newman'
                }
            }
        }

        stage('Run Postman Tests') {
            steps {
                script {
                    echo 'Running Postman tests'
                    
                    // Run Postman tests using npx (bypassing global installation)
                    bat 'npx newman run "Jules AI.postman_collection.json" --color off --disable-unicode --reporters cli,json --reporter-json-export newman-report.json'
                }
            }
        }

        stage('Publish Test Results') {
            steps {
                script {
                    echo "Publishing test results"
                    // Archive the generated report as an artifact
                    archiveArtifacts artifacts: 'newman-report.json', allowEmptyArchive: true
                }
            }
        }
    }
    post {
        always {
            // Clean up or notify about pipeline status, if necessary
            echo 'Pipeline finished'
        }
        failure {
            // Handle failure notification or cleanup
            echo 'Pipeline failed'
        }
    }
}
