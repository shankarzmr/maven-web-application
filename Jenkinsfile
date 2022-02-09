node {
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    def mavenHome = tool name: "maven3.8.4"


stage('checkoutcode'){
 
git branch: 'development', credentialsId: 'e89b00a3-a9a8-4a52-a14f-fcded6e7f0ee', url: 'https://github.com/shankarzmr/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('Executesonarqubereport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('UploadArtifactintonexusrepo'){
sh "${mavenHome}/bin/mvn deploy"
}
stage('DeployappintoTomcatserver'){
sshagent(['985d71e3-d90a-4720-8787-e979aaae9726']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.121.108:/opt/apache-tomcat-9.0.58/webapps/"

    
}
}



} //node closed
