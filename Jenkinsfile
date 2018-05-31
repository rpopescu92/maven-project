pipeline {
    agent any

    tools {
        maven 'localMaven'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }

            post {
                success {
                    echo "Now archiving...."
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deploy to staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production') {
            steps{
                timeout(time:5, unit: 'DAYS') {
                    input message: 'approve production'
                }

                build job: 'deploy-to-prod'
            }

            post {
                success {
                    echo 'code deployed to prod'
                }
                failure {
                    echo 'deploy failed'
                }
            }
        }
    }
}