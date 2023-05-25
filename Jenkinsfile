pipeline{
    agent any
    environment {
//     siteUrl = "https://medtronic.sharepoint.com/sites/PAACDevOps-CarelinkConnect"
//     libraryName = "Documents"
//     filePath = "${WORKSPACE}/build/artifacts/Runner.ipa"
//     ZipfilePath = "${WORKSPACE}/zip/artifacts.zip"
    TEAMS_WEBHOOK = credentials("teams_webhook")
    }
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
                curl -H "Content-Type: application/json" -d '{"text": "Status: '"$BUILD_STATUS"'<br>Pipeline: [Jenkins build #'$BUILD_NUMBER']('$BUILD_URL')<br>Commit link: ['$GIT_COMMIT']('$GIT_REPO_URL'/-/commit/'$GIT_COMMIT')<br>App identifier: '$ENTERPRISE_BUNDLE_IDENTIFIER'<br>Branch name: '$BRANCH_NAME'<br>'"$LOG_FILES_URL"''"$app_center_upload_failure"''"$app_center_map"''"$sharepoint_map"''"$FORTIFY_SCAN_URL"''"$FORTIFY_PDF_URL"''"$IOS_NOWSECURE_PDF_URL"''"$ANDROID_NOWSECURE_PDF_URL"'"}' "$TEAMS_WEBHOOK"
                '''
                }

            }
        }

}
