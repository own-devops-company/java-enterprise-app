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
         
          sh 'docker build -t 942288870879.dkr.ecr.ap-south-1.amazonaws.com/javawebapp":$BUILD_NUMBER" . || true'              
    }
    
    stage('Login Docker Push'){

          docker.withRegistry('https://942288870879.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:AWS_Access') {
               docker.image('javawebapp').push('latest')
          }
        // sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 942288870879.dkr.ecr.ap-south-1.amazonaws.com'
        //sh "docker push 942288870879.dkr.ecr.ap-south-1.amazonaws.com/javawebapp:$BUILD_NUMBER"       
     }
  /*   
      stage('Pull Image Run Container'){
         
         sshagent(['Dock_Dep']) {
          sh 'ssh -o StrictHostKeyChecking=no centos@13.127.83.158 docker stop java-web-app || true'
          sh 'ssh  centos@13.127.83.158 docker rm javawebapp || true'
          sh 'ssh  centos@13.127.83.158 docker rmi -f  $(docker images -q) || true'
          sh 'ssh  centos@13.127.83.158 '
       }
       
    }
     */
}
