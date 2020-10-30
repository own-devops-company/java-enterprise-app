node{
     
     def mavenHome = tool name: "maven3.6.3"
     
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
    
    stage ('GenerateSonarQubeReport')
    {
        sh "${mavenHome}/bin/mvn clean sonar:sonar"
    }

stage ('UploadArtifactNexus')
    {
        sh "${mavenHome}/bin/mvn deploy"
    }
  /*   
    stage('Build Docker Image'){
        sh 'docker build -t dockerhandson/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u dockerhandson -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push dockerhandson/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app dockerhandson/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.20.72 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.20.72 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.20.72 ${dockerRun}"
       }
       
    }
    */ 
     
}
