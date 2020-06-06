node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/gauthamdharsan/playjenkins.git'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.3.9", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
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
     
     stage("Deploy To Kuberates Cluster"){
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
