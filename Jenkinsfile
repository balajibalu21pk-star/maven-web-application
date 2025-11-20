node 
{

def mavenHome = tool name: "maven3.9.11"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])

stage('checkout')
{
git branch: 'development', credentialsId: 'dbf35f04-af4c-4c98-8ec2-5023009e8104', url: 'https://github.com/balajibalu21pk-star/maven-web-application.git'
}    

stage('build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}
 
stage('UploadArtiFactsintoNexus')
{
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployAppintoTomcatServer')
{
sshagent(['709b6e54-7c97-4f4a-8529-97e3ac723ba1']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@13.200.10.64:/opt/tomcat/webapps/" 
}
}
stage('SendEmailNotification')
{
emailext body: '''Build Over..

Regards,
Balaji.
''', subject: 'Build Over', to: 'balajibalu21pk@gmail.com'
}

}
