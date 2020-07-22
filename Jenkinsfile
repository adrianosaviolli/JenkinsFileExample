def gitURL = "https://github.com/adrianosaviolli/SampleWithTests"
node {
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
