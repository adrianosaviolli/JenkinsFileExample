
def gitURL = "https://github.com/adrianosaviolli/SampleWithTests"

pipeline {
    agent any
    triggers { pollSCM('*/1 * * *') }
    stages {
        stage('Poll SCM') {
            steps {
                script {
                    properties([pipelineTriggers([pollSCM('')])])
                }
                git branch: "develop", url: 'https://github.com/adrianosaviolli/SampleWithTests'
            }
        }

        stage('Prepare') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: "develop"]],extensions: [[$class: 'CloneOption', shallow: true, timeout: 50]],userRemoteConfigs: [[url: "$gitURL"]]])
            }
        }

        stage('Build') {
            steps {
                sh "./gradlew assembleDebug"
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/*.apk', onlyIfSuccessful: true
            }
        }
    }
}