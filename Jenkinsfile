
def gitURL = "https://github.com/adrianosaviolli/SampleWithTests"

pipeline {
    stages {
        stage('Poll SCM') {
            script {
                properties([pipelineTriggers([pollSCM('')])])
            }
            git branch: "develop", url: 'https://github.com/adrianosaviolli/SampleWithTests'
        }

        stage('Prepare') {
            deleteDir()
        }

        stage('Checkout') {
            checkout(
                [$class: 'GitSCM', branches: [[name: "develop"]],
                extensions: [[$class: 'CloneOption', shallow: true, timeout: 50]],
                userRemoteConfigs: [[url: "$gitURL"]]]
            )
        }

        stage('Build') {
            sh "./gradlew assembleDebug"
        }

        stage('Archive') {
            archiveArtifacts artifacts: '**/*.apk', onlyIfSuccessful: true
        }
    }
}