pipeline{
    agent any
//     environment {
//     siteUrl = "https://medtronic.sharepoint.com/sites/PAACDevOps-CarelinkConnect"
//     libraryName = "Documents"
//     filePath = "${WORKSPACE}/build/artifacts/Runner.ipa"
//     ZipfilePath = "${WORKSPACE}/zip/artifacts.zip"
//     TEAMS_WEBHOOK = credentials("teams_webhook")
//     }
    stages{
        stage('Build') {
            steps {
                load "${WORKSPACE}/Jenkinsfile-vars.groovy"
                sh 'flutter build ios --release'
                sh 'xcodebuild -quiet build -workspace $WORKSPACE/ios/Runner.xcworkspace -scheme Runner -sdk iphoneos -configuration Release clean archive -archivePath build/Runner.xcarchive'
                sh 'xcodebuild -exportArchive -archivePath build/Runner.xcarchive -exportPath build/Users/zmo-mac-akashh-01/Desktop/Runner.ipa -exportOptionsPlist ExportOptions.plist'
                }
            }
        stage("team notification"){
            steps {
                script {
                sh '''
                curl -H 'Content-Type: application/json' -d '{"text": "Status: '$BUILD_NUMBER' "}' 'https://zimetrics0.webhook.office.com/webhookb2/86c019d7-0a06-4ba4-bc4a-8250064556ff@49a9b2be-8e5c-4716-82b5-b8b8f942f368/IncomingWebhook/325b828515774b6ba8ee7880d1534ec7/0d88e168-0bc8-49b2-9bca-f43506c04073''''
                '''}

            }
        }
    }
}
