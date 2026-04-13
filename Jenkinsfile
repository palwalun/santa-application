pipeline{
agent any
  environment{
  ACR_LOGIN_SERVER = 'devopsproject2.azurecr.io'
  IMAGE_NAME = 'santa-application'
  TAG = 'latest'
 }
 stages{
   stage('Checkout'){
    steps{
	 git url: 'https://github.com/palwalun/santa-application.git',
	 branch: 'master'
	}
   }
  stage('Build'){
   steps{
    sh 'mvn clean package -DskipTests'
   }
  }
 stage('Build Image'){
  steps{
   sh 'docker build -t santa-application .'
  }
 }
  stage('Login to ACR') {
       steps {
         withCredentials([usernamePassword(
             credentialsId: 'acr-creds',
             usernameVariable: 'ACR_USER',
             passwordVariable: 'ACR_PASS'
         )]) {
             sh '''
               echo $ACR_PASS | docker login $ACR_LOGIN_SERVER \
               -u $ACR_USER --password-stdin
             '''
           }
          }
         }
  stage('Tag Image') {
        steps {
         sh '''
           docker tag ${IMAGE_NAME}:${TAG} \
           $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}
         '''
          }
        }
  stage('Push Image to ACR'){
	      steps{
	        sh 'docker push $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}'
	    }
	   }
  stage('Deploy to K8s'){
	 steps{
	  sh 'kubectl apply -f deployment.yml'
	 }
	
	}
 
 }

}
