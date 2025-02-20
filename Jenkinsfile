pipeline {
    agent any

    triggers {
        cron('H/3 * * * 4')  // Runs every 3 minutes on Thursdays
    }

    environment {
        JAVA_HOME = '/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home'
        PATH = "${JAVA_HOME}/bin:${PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SujanChaudhari/spring-petclinic.git'
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

