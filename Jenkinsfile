pipeline{
agent any
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
   docker build -t santa-application .
  }
 }
 }

}