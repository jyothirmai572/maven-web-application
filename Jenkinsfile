node {
    
def mavenHome = tool name: "maven 3.8.4"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

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

stage('DeployAppIntoTomcatServer')
{
sshagent(['12845092-ffb5-4c7d-acc7-df0a45b361d1']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.35.226:/opt/apache-tomcat-9.0.63/webapps"
} 
}


}
