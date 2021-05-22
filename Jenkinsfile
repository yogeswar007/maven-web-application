node
{
 
  def mavenHome = tool name: "maven3.8.1"
  
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
  
  stage('CheckoutCode')
  {
  git branch: 'development', credentialsId: 'e373d4ed-d201-4e75-96e6-99ea433d5620', url: 'https://github.com/yogeswar007/maven-web-application.git'
  }
  
  stage('Build')
  {
  sh "${mavenHome}/bin/mvn clean package"
  }
 
  stage('ExecuteSonarQubeReport')
  {
  sh "${mavenHome}/bin/mvn sonar:sonar"
  }
  
  stage('UploadArtifactsintoNexus')
  {
  sh "${mavenHome}/bin/mvn deploy"
  } 
  
  stage('DeployAppintoTomcatServer')
  {
  sshagent(['1a6aff83-3ea5-417a-9014-bda1ae04b15b']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.90.77:/opt/apache-tomcat-9.0.45/webapps/"
  }
  }
  
  stage('SendEmailNotification')
  {
  emailext body: '''Build Over...

  Regards,
  Yogeswar''', subject: 'Build Over...', to: 'yogeswargajula007@gmail.com'
  }
}
