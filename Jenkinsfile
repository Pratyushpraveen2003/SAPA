pipeline {
    agent any

    environment {
        // Environment variables (e.g., API credentials, paths)
        API_URL = 'https://your-api-url.com/user-data'
        REPORT_PATH = 'user_reports'
    }

    stages {
        stage('Setup') {
            steps {
                script {
                    // Create report directory if it doesn't exist
                    sh "mkdir -p ${REPORT_PATH}"
                }
            }
        }

        stage('Fetch User Data') {
            steps {
                script {
                    // Fetch user data from an API or another source
                    echo 'Fetching user data...'
                    def response = httpRequest(
                        url: "${API_URL}",
                        httpMode: 'GET',
                        customHeaders: [
                            [name: 'Authorization', value: 'Bearer your-api-token']
                        ]
                    )
                    writeFile file: "${REPORT_PATH}/user_data.json", text: response.content
                }
            }
        }

        stage('Process User Data') {
            steps {
                script {
                    // Assuming user_data.json is a JSON file with user data to process
                    echo 'Processing user data...'
                    def userData = readJSON file: "${REPORT_PATH}/user_data.json"

                    // Example: Process each user and log their activity
                    userData.each { user ->
                        echo "Processing user: ${user.name} (ID: ${user.id})"
                        // Here you could add logic to analyze user data, e.g., activity frequency
                    }

                    // Save a summary report as an example
                    writeFile file: "${REPORT_PATH}/user_summary.txt", text: "User summary report generated on ${new Date()}"
                }
            }
        }

        stage('Report User Activity') {
            steps {
                script {
                    // Example: Archive the report for future reference
                    archiveArtifacts artifacts: "${REPORT_PATH}/user_summary.txt", allowEmptyArchive: true

                    // Send an email notification with the report if needed
                    emailext(
                        to: 'you@example.com',
                        subject: "User Activity Report - ${new Date()}",
                        body: "The user activity report is attached.",
                        attachLog: true,
                        attachmentsPattern: "${REPORT_PATH}/user_summary.txt"
                    )
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after job completion
            deleteDir()
        }
    }
}
