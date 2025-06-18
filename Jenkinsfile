pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'gradlew.bat build'
            }
        }

        stage('Test') {
            steps {
                bat 'gradlew.bat test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/build/libs/*.jar', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Lọc bỏ file -plain.jar, chỉ lấy JAR thực thi chính
                    def jarFiles = findFiles(glob: 'build/libs/*.jar').findAll { !it.name.contains('-plain') }

                    if (jarFiles.isEmpty()) {
                        error "❌ No valid JAR file found to deploy"
                    }

                    def jarPath = jarFiles[0].path
                    echo "🚀 Deploying JAR: ${jarPath}"
                    bat "java -jar ${jarPath}"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Jenkins pipeline completed successfully!'
        }
        failure {
            echo '❌ Jenkins pipeline failed.'
        }
    }
}
