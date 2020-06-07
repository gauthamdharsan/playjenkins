node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/gauthamdharsan/playjenkins.git'
    }
     
    stage('Build Docker Image'){
        sh 'docker build -t gauthamdharsan/myweb:1 .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'dockerpass', variable: 'dockerpass')]) {
          sh "docker login -u gauthamdharsan -p ${dockerpass}"
        }
        sh 'docker push gauthamdharsan/myweb:1'
     }
     
     stage("Deploy To Kubernetes Cluster"){
       kubernetesDeploy(
         configs: 'myweb.yaml', 
         kubeconfigId: 'kube-config',
         enableConfigSubstitution: true
        )
     }
	 
	  
      stage("Deploy To Kuberates Cluster"){
        sh 'kubectl apply -f myweb.yaml'
      } 
     
}
