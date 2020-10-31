node{
     
     def mavenHome = tool name: "maven3.6.3"
     def giturl = "https://github.com/own-devops-company/java-webapp-docker.git"
     
     echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
     
    stage('SCM Checkout'){
        git branch: 'master', credentialsId: '65fb834f-a83b-4fe7-8e11-686245c47a65', url: 'https://github.com/own-devops-company/java-webapp-docker.git'
    }
    
    stage(" Maven Clean Package"){
      sh "${mavenHome}/bin/mvn clean package"
      } 
/*    
    stage ('GenerateSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }

stage ('UploadArtifactNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
     */
    stage('Build Docker Image'){
         
         sshagent(['Dok-Img']) {
          sh 'ssh -o StrictHostKeyChecking=no centos@3.7.45.160 docker build -t saianuroop/javawebapp":$BUILD_NUMBER" "${giturl}" || true'
       }        
    }
    
    stage('Push Docker Image'){
         
         sshagent(['Dok-Img']) {
           withDockerRegistry(credentialsId: 'Dok-Cred', url: 'https://hub.docker.com/') {
          sh "ssh centos@3.7.45.160 docker login -u saianuroop -p ${Dok-Cred}"          
        }
          sh 'ssh centos@3.7.45.160 docker push saianuroop/javawebapp* || true'      
       }             
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = 'docker run  -d -p 7070:8080 --name java-web-app saianuroop/javawebapp*'
         
         sshagent(['Dock_Dep']) {
          sh 'ssh -o StrictHostKeyChecking=no centos@13.127.83.158 docker stop java-web-app || true'
          sh 'ssh  centos@13.127.83.158 docker rm java-web-app || true'
          sh 'ssh  centos@13.127.83.158 docker rmi -f  $(docker images -q) || true'
          sh "ssh  centos@13.127.83.158 ${dockerRun}"
       }
       
    }
     
}
