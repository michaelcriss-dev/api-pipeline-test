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
                sh '''
                npm install newman-reporter-htmlextra
                npm install newman
                '''
            }
        }

        stage('Run API Tests') {
            steps {
                sh '''
                    npx newman run collections/collection-requests.json \
                      --reporters cli,junit,htmlextra \
                      --reporter-htmlextra-export reports/newman.html \
                      --reporter-junit-export reports/newman.xml

                '''
            }
        }

        stage('Archieve Reports') {
            steps {
                archiveArtifacts artifacts: 'reports/*.html, reports/*.xml', fingerprint: true
                junit 'reports/newman.xml'
            }
        }
    }
}
