@Library('sharedlbsmt') _
node {
    
def mavenHome = tool name: "maven 3.8.4"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
echo "The job name is: ${env.JOB_NAME}"
    try{
sendSlackNotifications('STARTED')
stage('Checkout code')
{
git branch: 'development', credentialsId: 'd7ccb245-3404-4d9b-a469-f6537133f4f6', url: 'https://github.com/jyothirmai572/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
        
stage('Execute SonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadingArtifactstoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
          
    
/*
stage('DeployAppIntoTomcatServer')
{
sshagent(['12845092-ffb5-4c7d-acc7-df0a45b361d1']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.35.226:/opt/apache-tomcat-9.0.63/webapps"
} 
}
    }catch(e){
       currentBuild.result = "FAILED"
        throw e
    }
    finally{
        sendSlackNotifications(currentBuild.result)
    }
*/
}
/*
def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESSFUL'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESSFUL') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: 'walmart')
}
*/
}
