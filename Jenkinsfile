node{
   stage('SCM Checkout'){
     git 'https://github.com/alhajtechnovalley/my-app.git'
   }
   stage('Build-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
    stage('Build Docker Imager'){
   sh 'docker build -t alhaj1986/myprojweb:0.0.1 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u alhaj1986 -p ${dockerPassword}"
    }
   sh 'docker push alhaj1986/myprojweb:0.0.1'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 43.205.210.216:8083"
   sh "docker tag alhaj1986/myprojweb:0.0.1 43.205.210.216:8083/damo:1.0.0"
   sh 'docker push 43.205.210.216:8083/damo:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest alhaj1986/myprojweb:0.0.1' 
   }
}
}
