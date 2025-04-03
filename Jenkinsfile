pipeline {
    agent {
        docker {
            image 'maven:3.8.6-openjdk-11'
        }
    }

    stages {


        stage('Run Selenium Tests') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Run JMeter Tests') {
            steps {
                sh 'mvn jmeter:jmeter'
            }
        }

        stage('Generate JMeter Report') {
            steps {
                sh 'jmeter -g target/jmeter/results/results.jtl -o target/jmeter/reports'
            }
        }

        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'target/jmeter/results/results.jtl, target/jmeter/reports/**', fingerprint: true
            }
        }

        stage('Publish Performance Report') {
            steps {
                performanceReport sourceDataFiles: 'target/jmeter/results/results.jtl', parsers: [jMeter()]
            }
        }

        stage('Publish JMeter HTML Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'target/jmeter/reports',
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
