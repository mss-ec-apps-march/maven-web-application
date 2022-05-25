node
{
    def mavenHome = tool name: "Maven 3.6.3"
    
 stage('CheckoutCode')
  {
   git branch: 'development', credentialsId: 'c5c086e1-00e8-4088-9703-284d920320fc', url: 'https://github.com/mss-ec-apps-march/maven-web-application.git'
  }

 stage('Build')
  {
   sh "${mavenHome}/bin/mvn clean package" 
   }
   stage('ExecuteSonarQubeReport')
   {
    sh "${mavenHome}/bin/mvn sonar:sonar" 
    }

 stage('UploadArtifactintoNexus')
  {
  sh "${mavenHome}/bin/mvn deploy" 
  }

 stage('DeployAppIntoTomcatServer')
  {
   sshagent(['1c856ed9-d694-4cea-a18c-65690e1a5bd1']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.185.120:/opt/apache-tomcat-9.0.63/webapps/"
  }
 }

stage('SendEmailNotification')
  {
  emailext body: '''Build Over - Scriptedway


Regards,
Haseena Vaduguru,
8309492346''', subject: 'Build Over - Scriptedway', to: 'haseenavaduguru@gmail.com'
  }

}
