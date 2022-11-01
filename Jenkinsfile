node {
    stage('Checkout Data-Api') {
        git branch: 'main', url: 'https://github.com/antenehz/BAH-MCCProj-data-api.git'
    }
    
    stage('Gradle Build Data-Api') {
        sh 'gradle clean build'
    }
    
    stage('Gradle Boot-Jar Data-Api') {
        sh 'gradle bootjar'
    }
    
    stage('Containerize Data-Api') {
        sh 'docker build --rm -t data-api:v1.0 .'
    }
    
    stage('Inspect Docker Image') {
        sh 'docker images data-api:v1.0'
        sh 'docker inspect data-api:v1.0'
    }
    
    stage('Run Docker Instance') {
        sh 'docker stop data-api'
        sh 'docker run -d --rm --name data-api -p 8080:8080 data-api:v1.0'
    }
    
    stage('User Acceptance Test (Release to kube)') {
       def response= input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
	  if(response=="Yes") {
	    stage('Deploy to Kubenetes cluster - DataApi') {
	      sh 'kubectl delete service data-api'
	      sh 'kubectl delete deployment data-api'
	      sh 'kubectl create deployment data-api --image=data-api:v1.0'
	      sh 'kubectl expose deployment data-api --type=LoadBalancer --port=8080'
	    }
	  }
    }
    
     stage("Production Deployment View"){
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
}
