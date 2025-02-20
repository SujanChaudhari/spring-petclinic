pipeline {
    agent any

    triggers {
        cron('H/3 * * * 4')  // Runs every 3 minutes on Thursdays
    }

    environment {
        JAVA_HOME = '/usr/lib/jvm/java-11-openjdk' // Update this if needed
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR_GITHUB_USERNAME/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Test & Coverage') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Generate JaCoCo Report') {
            steps {
                sh './mvnw jacoco:report'
            }
            post {
                success {
                    jacoco execPattern: '**/target/jacoco.exec', 
                           classPattern: '**/target/classes', 
                           sourcePattern: '**/src/main/java', 
                           inclusionPattern: '**/*.java'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}

