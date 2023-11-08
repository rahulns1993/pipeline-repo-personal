node{
    
echo "jenkins URL is : ${env.JENKINS_URL}"
echo "NODE NAME is : ${env.NODE_NAME}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
def mavenHome = tool name:"maven3.9.5"
stage('checkout'){
    git credentialsId: 'd17fba64-0540-4f79-b38f-a095340538f4', url: 'https://github.com/rahulns1993/java-web-app-docker.git'
}


stage('Build'){
    sh "${mavenHome}/bin/mvn clean package"

}
stage('execute SonarQube Report'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('Upload into Nexus Artifactory Repo'){
    sh "${mavenHome}/bin/mvn clean deploy"
}
stage('Deploy to TomCat Server'){
sshagent(['SSHcredentials']) {
    sh "scp -o StrictHostKeyChecking=no target/java-web-app-1.0.war ec2-user@172.31.37.136:/opt/tomcat9/webapps/"
}
}

}
