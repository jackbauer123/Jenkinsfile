def label = "mypod-${UUID.randomUUID().toString()}"

podTemplate(label:label,cloud: "kubernetes",
    containers: [
    	containerTemplate(name: 'maven', image: 'maven:3.8.4-jdk-8', command: 'sleep', args: '99d'),
    	containerTemplate(name: 'docker', image: 'docker', command: 'sleep', args: '99d')]
	    ,
    volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
	   ) {
  node(label) {
		
		environment {
			registry = "YourDockerhubAccount/YourRepository"
			registryCredential = 'dockerhub_id'
			dockerImage = ''
		}
		stage('SCM') {
			//dir('/tmp'){
				git credentialsId: 'github', url: 'git@github.com:jackbauer123/mytest.git'
			//}
		}

		stage('Build') {
			container('maven') {
				sh 'mvn -B -ntp clean package -DskipTests'
			}
			
		}
	 
		  stage('build storage image') {
			  //dir('/tmp'){
				  container('docker'){
					  storage = docker.build("library/storage:${env.BUILD_ID}","./storage/Dockerfile")
				  }
			 // }
		  }
	  
	   stage('build account image') {
			  //dir('/tmp'){
				  container('docker'){
					  account = docker.build("library/account:${env.BUILD_ID}","./account/Dockerfile")
				  }
			 // }
		  }
	  
	   stage('build order image') {
			  //dir('/tmp'){
				  container('docker'){
					  order = docker.build("library/order:${env.BUILD_ID}","./order/Dockerfile")
				  }
			 // }
		  }
	  
	  
	   stage('build logic image') {
			  //dir('/tmp'){
				  container('docker'){
					  logic = docker.build("library/logic:${env.BUILD_ID}","./logic/Dockerfile")
				  }
			 // }
		  }
	  
	  
	  
		  stage('Push image') {
			  container('docker'){
				docker.withRegistry('https://harbor.yuanzhibin.com', 'harbor-admin') {
							storage.push("${env.BUILD_NUMBER}")
							storage.push("latest")
					
					account.push("${env.BUILD_NUMBER}")
							account.push("latest")
					
					order.push("${env.BUILD_NUMBER}")
							order.push("latest")
					
					logic.push("${env.BUILD_NUMBER}")
							logic.push("latest")
				} 

			  }	  
		 }

		
    }

}

