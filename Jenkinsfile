node{
  def mavenHome = tool name: 'maven3.9.5'
  
  echo "the job name is : ${env.JOB_NAME}"
echo "Jenkins home dir is : ${env.JENKINS_HOME}"
echo "Jenkins NODE name is : ${env.NODE_NAME}"
echo "the build number is : ${env.BUILD_NUMBER}"
  
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'RebuildSettings', autoRebuild: false, rebuildDisabled: false], pipelineTriggers([pollSCM('* * * * *')])])
stage('CheckoutCode'){
git branch: 'development', credentialsId: '35135c3d-aaf4-4f08-a6fd-442559ebdb67', url: 'https://github.com/Sandeepm1193/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"

}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"

}

stage('UploadArtifactIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"

}

stage ('DeployAppIntoTomcat'){

sshagent(['e834c468-dc04-43ba-bbe7-2b129a183069']) {
 sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.40.95:/opt/apache-tomcat-9.0.93/webapps/"
}

}
}
