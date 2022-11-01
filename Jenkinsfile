node {
    
    stage ("Checkout Auth Service"){
        git branch: 'main', url: 'https://github.com/abarbuzza/BAH-MCC-Proj-Auth.git'
    }
    
    stage ("Gradle Build") {
        sh 'gradle clean build'
    }
    
    stage ("Gradle Bootjar") {
        sh 'gradle bootJar'
    }
    
    stage ("Containerize the app with Docker") {
        sh 'docker build --rm -t event-auth:v1.0 .'
    }
    
    stage ("Inspect the docker image"){
        sh "docker images event-auth:v1.0"
        sh "docker inspect event-auth:v1.0"
    }
    
    stage ("Run Docker container instance"){
	sh "docker stop event-auth"
        sh "docker run -d --rm --name event-auth -p 8081:8081 event-auth:v1.0"
     }
    
    stage('User Acceptance Test') {
        def response= input message: 'Is this build good to go?',
        parameters: [choice(choices: 'Yes\nNo',
        description: '', name: 'Pass')]
        if(response=="Yes") {
          stage('Deploy to Kubenetes cluster') {
		sh "kubectl delete service event-auth"
		sh "kubectl delete deployment event-auth"
	        sh "kubectl create deployment event-auth --image=event-auth:v1.0"
		    //get the value of API_HOST from kubernetes services and set the env variable
	        sh "set env deployment/event-auth API_HOST=\$(kubectl get service/event-data -o jsonpath='{.spec.clusterIP}'):8080"
	        sh "kubectl expose deployment event-auth --type=LoadBalancer --port=8081"
	        }
        }
    }
    
    stage("Production Deployment View"){
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
    
}
