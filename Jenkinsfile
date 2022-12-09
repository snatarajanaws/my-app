node{
   stage('SCM Checkout'){
     git 'https://github.com/snatarajanaws/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
	    stage('Build Docker Imager'){
   sh 'docker build -t snatarajanaws/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u snatarajanaws -p ${dockerPassword}"
    }
   sh 'docker push snatarajanaws/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p Tamilanda@86 43.204.22.148:8083"
   sh "docker tag snatarajanaws/myweb:0.0.2 43.204.22.148:8083/damo:1.0.0"
   sh 'docker push 43.204.22.148:8083/damo:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
	stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest snatarajanaws/myweb:0.0.2' 
   }
   }
   }
