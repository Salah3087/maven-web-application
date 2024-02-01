node{

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * *  *')])])

echo "Build Number is: ${env.BUILD_NUMBER}"
echo "Job Name is: ${env.JOB_NAME}"
echo "Node name is ${env.NODE_NAME}"
echo "Jenkins home dir is : ${env.JENKINS_HOME}"
echo "Build URL : ${env.BUILD_URL}"
echo "Jenkins URL is : ${env.JENKINS_URL}"



def mavenHome =tool name: 'Maven3.9.6'

stage('CheckOutCode'){
git branch: 'development', credentialsId: 'f4ab8816-daae-433a-92af-ee0b33fc8492', url: 'https://github.com/Salah3087/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsIntoNexus'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppintoTomcat'){
sshagent(['9bd6b108-ef63-4c93-8455-3c4d73a0a409']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.20.75:/opt/apache-tomcat-9.0.83/webapps/"
}
}

}
