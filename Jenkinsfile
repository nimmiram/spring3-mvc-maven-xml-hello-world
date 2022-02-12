pipeline{
    agent any
    stages{
      stage("scm"){
         steps{
              git credentialsId: 'github_clone', url: 'https://github.com/nimmiram/spring3-mvc-maven-xml-hello-world.git'
                  }
      }
         stage("build"){
             steps{
                 sh 'mvn package'
             }
         }
     stage("Artifact"){
             steps{
                 archiveArtifacts artifacts: 'target/*.war'
             }
         }
          stage("deploy"){
             steps{
                 withCredentials([usernameColonPassword(credentialsId: 'tomcat-credentials', variable: 'tomcatcred')]) {
                       sh "curl -v -u $tomcatcred admin:admin -T /var/lib/jenkins/workspace/spring3-mvn-hello-world/target/spring3-mvc-maven-xml-hello-world-1.0-SNAPSHOT.war 'http://ec2-3-7-65-209.ap-south-1.compute.amazonaws.com:8080/manager/text/deploy?path=/tomcat-pipeline&update=true'" 
            
                     
                 }
            
}
}
}
post {
        failure {
            script {
                currentBuild.result = 'FAILURE'
            }
        }

        always {
            step([$class: 'Mailer',
                notifyEveryUnstableBuild: true,
                recipients: "khushalreddyvundela@gmail.com,divyapathakota@gmail.com",
                sendToIndividuals: true])
        }
    }
}
