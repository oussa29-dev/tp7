pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Test') {
            agent any
            steps {
                script {
                    if (isUnix()) {
                        sh './gradlew clean test jacocoTestReport'
                    } else {
                        bat '.\\gradlew.bat clean test jacocoTestReport'
                    }
                }
            }
            post {
                always {
                    // Publish JUnit results
                    junit 'build/test-results/**/*.xml'
                    // Archive raw reports (Cucumber JSON, HTML reports and JaCoCo)
                    archiveArtifacts artifacts: 'build/reports/cucumber/**, build/reports/tests/**, build/reports/jacoco/**', allowEmptyArchive: true
                    // Optionally publish HTML reports if publishHTML plugin is installed
                    publishHTML(target: [
                        reportDir: 'build/reports/jacoco/test/html',
                        reportFiles: 'index.html',
                        reportName: 'JaCoCo Coverage',
                        allowMissing: true
                    ])
                    publishHTML(target: [
                        reportDir: 'build/reports/tests/test',
                        reportFiles: 'index.html',
                        reportName: 'Test Results',
                        allowMissing: true
                    ])
                }
            }
        }
    }
}
