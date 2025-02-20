pipeline {
    agent any

    environment {
    JAVA_HOME = '/Library/Java/JavaVirtualMachines/jdk-21.jdk/Contents/Home'
    PATH = "${JAVA_HOME}/bin:/opt/homebrew/bin:/opt/homebrew/Cellar/maven/3.9.9/bin:${PATH}"
}

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/SujanChaudhari/spring-petclinic.git', branch: 'main'
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
                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'JaCoCo Code Coverage Report'
                    ])
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed! Check the console output for errors."
        }
        success {
            echo "✅ Build and tests completed successfully!"
        }
    }
}

