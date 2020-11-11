node{
     def app
     def buildNumber = BUILD_NUMBER
     def mavenHome = tool name: "maven3.6.3"
     def giturl = "https://github.com/own-devops-company/java-webapp-docker.git"
     
      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
     
    stage('SCM Checkout'){
         git branch: 'master', credentialsId: '65fb834f-a83b-4fe7-8e11-686245c47a65', url: "${giturl}"
    }
    
    stage(" Maven Clean Package"){
      sh "${mavenHome}/bin/mvn clean package"
      } 
     
      stage('Build Docker Image') {
          app = docker.build "942288870879.dkr.ecr.ap-south-1.amazonaws.com/javawebapp:${buildNumber}"
      }
    
      stage('Push image') {
          docker.withRegistry('https://942288870879.dkr.ecr.ap-south-1.amazonaws.com/javawebapp','ecr:ap-south-1:awsCred') {
              app.push("${buildNumber}")
          }
      }

      stage('Pull Image Run Container'){
         sshagent(['Dok-Dep']) {      
          sh 'ssh centos@13.127.52.177 docker stop javawebapp || true'
          sh 'ssh centos@13.127.52.177 docker rmi -f  $(docker images -q)'
          sh "ssh centos@13.127.52.177 'eval \$(aws ecr get-login --no-include-email --region ap-south-1)'"
          sh 'ssh centos@13.127.52.177 docker rm javawebapp || true'
          sh "ssh centos@13.127.52.177 docker pull 942288870879.dkr.ecr.ap-south-1.amazonaws.com/javawebapp:${buildNumber}"
          sh 'ssh centos@13.127.52.177 docker run -d -p 8080:8080 --name javaapps 942288870879.dkr.ecr.ap-south-1.amazonaws.com/javawebapp'
       }       
    }
}
