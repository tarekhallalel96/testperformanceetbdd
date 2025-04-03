pipeline {
    agent {
        docker {
            image 'maven:3.8.6-openjdk-11'
        }
    }

    environment {
        JMETER_RESULTS = 'results.jtl'
        JMETER_REPORTS = 'reports'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ton-repo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Run Selenium Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Run JMeter Tests') {
            steps {
                sh 'mvn jmeter:jmeter'
            }
        }

        stage('Generate JMeter Report') {
            steps {
                sh "jmeter -g ${JMETER_RESULTS} -o ${JMETER_REPORTS}"
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'results.jtl, reports/**', fingerprint: true
            }
        }

        stage('Publish Performance Report') {
            steps {
                performanceReport parsers: [jMeter('results.jtl')]
            }
        }

        stage('Publish JMeter HTML Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'index.html',
                    reportName: 'JMeter Test Report'
                ])
            }
        }
    }

    post {
        always {
            echo "Pipeline terminé, vérifiez les résultats sur Jenkins."
        }
    }
}
