node {
    
    stage ("Checkout React Client"){
        git branch: 'main', url: 'https://github.com/tjmuscles/mccfinalreact.git'
    }
    
    stage ("Install dependencies - react client") {
        sh 'npm install'
    }
    
    stage ("Containerize the app-docker build - react client") {
        sh 'docker build --rm -t mcc-react:v1.0 .'
        sh 'minikube image load mcc-react:v1.0
    }
    
    stage ("Inspect the docker image - react client"){
        sh "docker images mcc-react:v1.0"
        sh "docker inspect mcc-react:v1.0"
    }
    
    stage ("Run Docker container instance - react client"){
        sh "docker run -d --rm --name mcc-react -p 3000:3000 mcc-react:v1.0"
     }
    
    stage('User Acceptance Test - react client') {
	
	  def response= input message: 'Is this build good to go?',
	   parameters: [choice(choices: 'Yes\nNo', 
	   description: '', name: 'Pass')]
	
	  if(response=="Yes") {
	    stage('Deploy to Kubenetes cluster - react client') {
			sh "docker stop mcc-react"
			sh "kubectl create deployment mcc-react --image=mcc-react:v1.0"
			sh "kubectl expose deployment mcc-react --type=LoadBalancer --port=80"
			sh "kubectl set env deployment/mcc-react REACT_APP_AUTH_IP=mcc-auth:8081"
    	    sh "kubectl set env deployment/mcc-react REACT_APP_API_IP=mcc-data:8080"   
	    }
	  }
    }

    stage("Production Deployment View"){
        sh "kubectl get deployments"
        sh "kubectl get pods"
        sh "kubectl get services"
    }
}
