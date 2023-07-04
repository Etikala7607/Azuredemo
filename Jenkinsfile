import groovy.json.JsonSlurper

def getFtpPublishProfile(def publishProfilesJson) {
  def pubProfiles = new JsonSlurper().parseText(publishProfilesJson)
  for (p in pubProfiles)
    if (p['publishMethod'] == 'FTP')
      return [url: p.publishUrl, username: p.userName, password: p.userPWD]
}

node {
  withEnv(['AZURE_SUBSCRIPTION_ID=<09596d8b-933e-4093-a627-78cef0237f46>',
        'AZURE_TENANT_ID=<e21a08cd-c175-4061-93ab-754d019a3ec8>']) {
    stage('init') {
      checkout scm
    }
  
    stage('build') {
      sh 'mvn clean package'
    }
  
    stage('deploy') {
      def resourceGroup = '<my_resource>'
      def webAppName = '<Azuredemmm>'
      // login Azure
      withCredentials([usernamePassword(credentialsId: 'c63761b5-6a41-44ae-b5d9-bb47f0d03f47', passwordVariable: ' XXj8Q~b.wTDBcq_8PP9s3J72vN5nT2A6Yw9ZGaY', usernameVariable: '9a3398f8-c38b-460f-a78c-713b02f27526')]) {
       sh '''
          az login --service-principal -u $9a3398f8-c38b-460f-a78c-713b02f27526 -p $ XXj8Q~b.wTDBcq_8PP9s3J72vN5nT2A6Yw9ZGaYj -t $e21a08cd-c175-4061-93ab-754d019a3ec8
          az account set -s $AZURE_SUBSCRIPTION_ID
        '''
      }
      // get publish settings
      def pubProfilesJson = sh script: "az webapp deployment list-publishing-profiles -g $resourceGroup -n $webAppName", returnStdout: true
      def ftpProfile = getFtpPublishProfile pubProfilesJson
      // upload package
      sh "curl -T target/calculator-1.0.war $ftpProfile.url/webapps/ROOT.war -u '$ftpProfile.username:$ftpProfile.password'"
      // log out
      sh 'az logout'
    }
  }
}
