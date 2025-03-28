pipeline {
    agent { label 'agent1' }

    tools {
        git 'Default'
    }

    stages {
        stage('Prepare Environment') {
            steps {
                script {
                    sh 'chmod +x ./gradlew'
                }
            }
        }
        stage('Checkstyle Main') {
            steps {
                script {
                    sh './gradlew checkstyleMain'
                }
            }
        }
        stage('Checkstyle Test') {
            steps {
                script {
                    sh './gradlew checkstyleTest'
                }
            }
        }
        stage('Compile') {
            steps {
                script {
                    sh './gradlew compileJava'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh './gradlew test'
                }
            }
        }
        stage('JaCoCo Report') {
            steps {
                script {
                    sh './gradlew jacocoTestReport'
                }
            }
        }
        stage('JaCoCo Verification') {
            steps {
                script {
                    sh './gradlew jacocoTestCoverageVerification'
                }
            }
        }
    }
        post {
                always {
                    script {
                        def buildInfo = "Build number: ${currentBuild.number}\n" +
                                        "Build status: ${currentBuild.currentResult}\n" +
                                        "Started at: ${new Date(currentBuild.startTimeInMillis)}\n" +
                                        "Duration so far: ${currentBuild.durationString}"
                        telegramSend(message: buildInfo)
                    }
                }
                success {
                            script {
                                telegramSend(chatId: '8166438409', message: "Build succeeded! 🎉\n${env.BUILD_URL}")
                            }
                        }
                        failure {
                            script {
                                telegramSend(chatId: '8166438409', message: "Build failed! ❌\n${env.BUILD_URL}")
                            }
                        }
                        unstable {
                            script {
                                telegramSend(chatId: '8166438409', message: "Build is unstable! ⚠️\n${env.BUILD_URL}")
                            }
                        }
                        changed {
                            script {
                                telegramSend(chatId: '8166438409', message: "Build status changed!\n${env.BUILD_URL}")
                            }
                        }
                    }
            }
}