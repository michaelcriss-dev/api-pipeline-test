pipeline {
    agent any
    stages {
        stage('Fetch Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Newman') {
            steps {
                sh 'npm install newman'
            }
        }

        stage('Run API Tests') {
            steps {
                sh '''
                    npx newman run collections/collection-requests.json \
                        --reporters cli,junit,html \
                        --reporter-html-export reports/newman.html \
                        --reporter-junit-export reports/newman.xml
                '''
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML (target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'newman.html',
                    reportName: 'Postman API Test Report'
                ])
            }
        }
    }
}
