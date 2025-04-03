pipeline {
    agent {
        docker {
            image 'maven:3.8.6-openjdk-11'
        }
    }


    stages {


        stage('Install Dependencies') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Run Selenium Tests') {
            steps {
                sh 'mvn clean compile'
                sh 'mvn jmeter:jmeter'
                
            }
        }
 

    }

    
}