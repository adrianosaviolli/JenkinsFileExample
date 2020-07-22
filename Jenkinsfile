def gitURL = "https://github.com/adrianosaviolli/SampleWithTests"
node {
    docker {
        label 'android'
        image 'bradesco-vsmobile/android'
        args '-u 0 -v android-build-gradle-cache:/home/gradle/.gradle -v android-build-sdk:/usr/local/android-sdk'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    parameters {
        string(name: 'branch', defaultValue: "$developBranch", description: 'Which branch do you wanna run?')
        choice(name: 'flavor', choices: ['app_unico', 'exclusive', 'prime', 'private', 'classic'], description: 'What flavor you need?')
        booleanParam(name: 'release', defaultValue: false, description: 'Debug ☐, Relese ☑')
        booleanParam(name: 'test', defaultValue: true, description: 'Run tests ☑')
        booleanParam(name: 'quality', defaultValue: true, description: 'Collect quality metrics ☑')
        booleanParam(name: 'build', defaultValue: true, description: 'Run build ☑')        
        booleanParam(name: 'distribute', defaultValue: false, description: 'Run distribute ☑')
    }
    stage('Poll SCM') {
        script {
            properties([pipelineTriggers([pollSCM('*/1 * * * *')])])
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